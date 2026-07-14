---
layout: page
title: 一元练习
permalink: /ledger/
description: 每家公司每月投入 $1，用长期记录代替短期判断。
noindex: true
---

{% assign ledger = site.data.investments %}
{% assign total_invested = 0.0 %}
{% assign total_market_value = 0.0 %}
{% for company in ledger.companies %}
  {% assign shares = 0.0 %}
  {% for snapshot in company.snapshots %}
    {% assign bought = ledger.monthly_contribution | times: 1.0 | divided_by: snapshot.price_usd %}
    {% assign shares = shares | plus: bought %}
  {% endfor %}
  {% assign latest = company.snapshots | last %}
  {% assign invested = company.snapshots.size | times: ledger.monthly_contribution %}
  {% assign market_value = shares | times: latest.price_usd %}
  {% assign total_invested = total_invested | plus: invested %}
  {% assign total_market_value = total_market_value | plus: market_value %}
{% endfor %}
{% assign total_profit = total_market_value | minus: total_invested %}
{% assign total_return = total_profit | times: 100.0 | divided_by: total_invested %}

<div class="ledger-summary" aria-label="投资练习汇总">
  <div>
    <span>累计投入</span>
    <strong>{% include usd.html value=total_invested %}</strong>
  </div>
  <div>
    <span>当前市值</span>
    <strong>{% include usd.html value=total_market_value %}</strong>
  </div>
  <div>
    <span>总收益率</span>
    <strong>{% if total_return > 0 %}+{% endif %}{{ total_return | round: 2 }}%</strong>
  </div>
</div>

<p class="ledger-update">数据更新于 {{ ledger.updated_at | date: "%Y.%m.%d" }} · 股价为个人记录值，并非实时行情或投资建议。</p>

<div class="table-scroll" tabindex="0" role="region" aria-label="公司投资收益表">
  <table class="ledger-table">
    <thead>
      <tr>
        <th scope="col">公司</th>
        <th scope="col">当前股价</th>
        <th scope="col">累计投入</th>
        <th scope="col">当前市值</th>
        <th scope="col">收益率</th>
      </tr>
    </thead>
    <tbody>
      {% for company in ledger.companies %}
        {% assign shares = 0.0 %}
        {% for snapshot in company.snapshots %}
          {% assign bought = ledger.monthly_contribution | times: 1.0 | divided_by: snapshot.price_usd %}
          {% assign shares = shares | plus: bought %}
        {% endfor %}
        {% assign latest = company.snapshots | last %}
        {% assign invested = company.snapshots.size | times: ledger.monthly_contribution %}
        {% assign market_value = shares | times: latest.price_usd %}
        {% assign profit = market_value | minus: invested %}
        {% assign return_rate = profit | times: 100.0 | divided_by: invested %}
        <tr>
          <th scope="row">
            {{ company.name }}
            <small>{{ company.ticker }}</small>
          </th>
          <td>{% include usd.html value=latest.price_usd %}</td>
          <td>{% include usd.html value=invested %}</td>
          <td>{% include usd.html value=market_value %}</td>
          <td>{% if return_rate > 0 %}+{% endif %}{{ return_rate | round: 2 }}%</td>
        </tr>
      {% endfor %}
    </tbody>
  </table>
</div>

## 计算方式

每月 1 日，以当天的美元股价投入 $1。某公司的累计持有份额为：

`累计份额 = Σ（每月投入 $1 ÷ 当月美元股价）`

页面使用最新一次记录的价格计算当前市值。收益率为：

`收益率 =（当前市值 − 累计投入）÷ 累计投入 × 100%`

这是一项关于纪律和判断的个人练习。它不构成投资建议。
