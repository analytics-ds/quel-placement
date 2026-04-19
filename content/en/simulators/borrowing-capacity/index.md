---
title: "French mortgage borrowing capacity simulator"
description: "Calculate your French mortgage borrowing capacity in 2026: max monthly payment, borrowable capital, usury rate check. Free, instant result, no sign-up."
translationKey: "simulator-borrowing-capacity"
lastmod: 2026-04-20
---

<div class="callout-enbref">
<div class="callout-enbref-title">In short</div>
<ol>
<li>Borrowing capacity capped at 35 percent of net income, insurance included</li>
<li>Calculation automatically includes existing monthly debt</li>
<li>Automatic check against the Q2 2026 French usury rate</li>
<li>Instant result, no sign-up, no cookie</li>
</ol>
</div>

<form class="simulator" id="simBorrow" novalidate>
<div class="simulator-grid">
<div class="sim-field">
<label for="revenu">Net monthly income (euros)</label>
<input type="number" id="revenu" value="3500" min="0" step="100" inputmode="numeric">
<small>Net salary or regular income</small>
</div>
<div class="sim-field">
<label for="charges">Current monthly debt (euros)</label>
<input type="number" id="charges" value="0" min="0" step="50" inputmode="numeric">
<small>Running loans (consumer, auto, etc.)</small>
</div>
<div class="sim-field">
<label for="apport">Down payment (euros)</label>
<input type="number" id="apport" value="20000" min="0" step="1000" inputmode="numeric">
<small>Savings available (excluding fees)</small>
</div>
<div class="sim-field">
<label for="duree">Loan term</label>
<select id="duree">
<option value="10">10 years</option>
<option value="15">15 years</option>
<option value="20" selected>20 years</option>
<option value="25">25 years</option>
</select>
<small>Amortisation period</small>
</div>
<div class="sim-field">
<label for="taux">Nominal rate (%)</label>
<input type="number" id="taux" value="3.8" step="0.05" min="0" max="10" inputmode="decimal">
<small>Bank rate, excluding insurance</small>
</div>
<div class="sim-field">
<label for="assurance">Borrower insurance rate (%)</label>
<input type="number" id="assurance" value="0.35" step="0.05" min="0" max="3" inputmode="decimal">
<small>Typical range: 0.15 to 0.60 percent</small>
</div>
</div>
<div class="simulator-actions">
<button type="button" class="sim-btn" id="btnCalc">Calculate</button>
</div>
</form>

<div class="simulator-results" id="simResults">
<div class="sim-result sim-result-main">
<span class="sim-result-label">Borrowable capital</span>
<strong class="sim-result-value" id="res-capital">-</strong>
</div>
<div class="sim-result sim-result-main">
<span class="sim-result-label">Total budget with down payment</span>
<strong class="sim-result-value" id="res-budget">-</strong>
</div>
<div class="sim-result">
<span class="sim-result-label">Max monthly payment (excl. insurance)</span>
<strong class="sim-result-value" id="res-mensualite">-</strong>
</div>
<div class="sim-result">
<span class="sim-result-label">Total interest cost</span>
<strong class="sim-result-value" id="res-interets">-</strong>
</div>
<div class="sim-result sim-result-taeg" id="res-usure-card">
<span class="sim-result-label">Estimated APR</span>
<strong class="sim-result-value" id="res-taeg">-</strong>
<span class="sim-result-check" id="res-usure">-</span>
</div>
</div>

<div class="callout-asavoir">
These results are indicative. They rely on the 35 percent maximum debt ratio rule (insurance included) and the usury rate in force for Q2 2026. Only a formal loan offer issued by a bank commits to a definitive rate.
</div>

## How the calculation works

The simulator applies three steps:

1. **Max monthly payment** = 35 percent of net income, less current debt
2. **Borrowable capital**: standard annuity formula with APR and term
3. **Usury rate check**: compares APR against Q2 2026 cap (5.19 percent for 20 years and more, 4.48 percent for 10 to 20 years, 4 percent for less than 10 years)

## Calculation limits

- Does not replace an official bank simulation
- Does not include notary fees (around 7 to 8 percent in existing properties, 2 to 3 percent in new builds)
- Does not include guarantee fees (surety, mortgage)
- Does not include possible brokerage fees

## Strategies to increase your borrowing capacity

- **Clear consumer loans**: a 300 euros monthly loan cuts 60,000 euros of borrowing capacity over 20 years
- **Delegate borrower insurance**: 30 to 50 percent savings, lower APR
- **Increase down payment**: reduces capital to borrow and reassures the bank
- **Lengthen the term**: increases capacity but also total cost

## To go further

- [Mortgage rates 2026: should you buy now?](/en/blog/mortgage-rates-2026/)
- [Q2 2026 usury rates](/en/blog/usury-rate-q2-2026/)
- [Borrower insurance: delegation or group contract?](/en/blog/borrower-insurance-delegation/)
- [Credit refinancing: when is it profitable in 2026?](/en/blog/credit-refinancing/)
- [Jeanbrun scheme: the new French private landlord status](/en/blog/jeanbrun-private-landlord-status/)

<script>
(function() {
var ids = ['revenu','charges','apport','duree','taux','assurance'];
var nf = new Intl.NumberFormat('en-GB', { style: 'currency', currency: 'EUR', maximumFractionDigits: 0 });
var usureByDuration = { 10: 4.00, 15: 4.48, 20: 5.19, 25: 5.19 };
function n(id) { return parseFloat(document.getElementById(id).value.toString().replace(',', '.')) || 0; }
function calc() {
var revenu = n('revenu'), charges = n('charges'), apport = n('apport');
var duree = parseInt(document.getElementById('duree').value, 10) || 20;
var taux = n('taux'), assurance = n('assurance');
var mensualiteMax = Math.max(0, revenu * 0.35 - charges);
var taeg = taux + assurance;
var r = taeg / 100 / 12, nMonths = duree * 12;
var capital = 0;
if (mensualiteMax > 0 && r > 0) { capital = mensualiteMax * (1 - Math.pow(1 + r, -nMonths)) / r; }
else if (r === 0) { capital = mensualiteMax * nMonths; }
var budget = capital + apport;
var interets = Math.max(0, mensualiteMax * nMonths - capital);
document.getElementById('res-capital').textContent = nf.format(capital);
document.getElementById('res-budget').textContent = nf.format(budget);
document.getElementById('res-mensualite').textContent = nf.format(mensualiteMax);
document.getElementById('res-interets').textContent = nf.format(interets);
document.getElementById('res-taeg').textContent = taeg.toFixed(2) + ' %';
var seuilUsure = usureByDuration[duree] || 5.19;
var usureCard = document.getElementById('res-usure-card');
var usureBadge = document.getElementById('res-usure');
if (taeg <= seuilUsure) {
usureBadge.textContent = 'Below the usury rate cap (' + seuilUsure.toFixed(2) + ' %)';
usureCard.classList.remove('sim-alert'); usureCard.classList.add('sim-ok');
} else {
usureBadge.textContent = 'Exceeds the usury rate cap (' + seuilUsure.toFixed(2) + ' %)';
usureCard.classList.remove('sim-ok'); usureCard.classList.add('sim-alert');
}
}
ids.forEach(function(id) { var el = document.getElementById(id); if (el) el.addEventListener('input', calc); });
var btn = document.getElementById('btnCalc'); if (btn) btn.addEventListener('click', calc);
calc();
})();
</script>
