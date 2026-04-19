---
title: "Simulateur de capacite d'emprunt immobilier"
description: "Calculez gratuitement votre capacite d'emprunt immobilier en 2026 : mensualite max, capital empruntable, verification du taux d'usure. Sans inscription, resultat instantane."
translationKey: "simulator-borrowing-capacity"
lastmod: 2026-04-20
---

<div class="callout-enbref">
<div class="callout-enbref-title">En bref</div>
<ol>
<li>Capacite d'emprunt plafonnee a 35 pour cent des revenus nets, assurance incluse</li>
<li>Le calcul integre automatiquement les charges mensuelles en cours</li>
<li>Check automatique du taux d'usure 2e trimestre 2026</li>
<li>Resultat instantane, sans inscription, sans cookie</li>
</ol>
</div>

<form class="simulator" id="simBorrow" novalidate>
  <div class="simulator-grid">
    <div class="sim-field">
      <label for="revenu">Revenu mensuel net (euros)</label>
      <input type="number" id="revenu" value="3500" min="0" step="100" inputmode="numeric">
      <small>Salaire net ou revenus reguliers</small>
    </div>

    <div class="sim-field">
      <label for="charges">Charges mensuelles existantes (euros)</label>
      <input type="number" id="charges" value="0" min="0" step="50" inputmode="numeric">
      <small>Credits en cours (conso, auto, etc.)</small>
    </div>

    <div class="sim-field">
      <label for="apport">Apport personnel (euros)</label>
      <input type="number" id="apport" value="20000" min="0" step="1000" inputmode="numeric">
      <small>Epargne mobilisable hors frais</small>
    </div>

    <div class="sim-field">
      <label for="duree">Duree du pret</label>
      <select id="duree">
        <option value="10">10 ans</option>
        <option value="15">15 ans</option>
        <option value="20" selected>20 ans</option>
        <option value="25">25 ans</option>
      </select>
      <small>Duree d'amortissement</small>
    </div>

    <div class="sim-field">
      <label for="taux">Taux nominal (%)</label>
      <input type="number" id="taux" value="3.8" step="0.05" min="0" max="10" inputmode="decimal">
      <small>Taux bancaire hors assurance</small>
    </div>

    <div class="sim-field">
      <label for="assurance">Taux d'assurance emprunteur (%)</label>
      <input type="number" id="assurance" value="0.35" step="0.05" min="0" max="3" inputmode="decimal">
      <small>Typique : 0,15 a 0,60 pour cent</small>
    </div>
  </div>
</form>

<div class="simulator-results" id="simResults">
  <div class="sim-result sim-result-main">
    <span class="sim-result-label">Capital empruntable</span>
    <strong class="sim-result-value" id="res-capital">-</strong>
  </div>
  <div class="sim-result sim-result-main">
    <span class="sim-result-label">Budget total avec apport</span>
    <strong class="sim-result-value" id="res-budget">-</strong>
  </div>
  <div class="sim-result">
    <span class="sim-result-label">Mensualite maximale (hors assurance)</span>
    <strong class="sim-result-value" id="res-mensualite">-</strong>
  </div>
  <div class="sim-result">
    <span class="sim-result-label">Cout total des interets</span>
    <strong class="sim-result-value" id="res-interets">-</strong>
  </div>
  <div class="sim-result sim-result-taeg" id="res-usure-card">
    <span class="sim-result-label">TAEG estime</span>
    <strong class="sim-result-value" id="res-taeg">-</strong>
    <span class="sim-result-check" id="res-usure">-</span>
  </div>
</div>

<div class="callout-asavoir">
Ces resultats sont indicatifs. Ils reposent sur la regle des 35 pour cent d'endettement maximum (assurance incluse) et le taux d'usure en vigueur au 2e trimestre 2026. Seule une offre de pret formelle emise par une banque engage sur un taux definitif.
</div>

## Comment fonctionne le calcul ?

Le simulateur applique trois etapes :

1. **Mensualite max** = 35 pour cent du revenu net, diminue des charges actuelles
2. **Capital empruntable** : formule d'annuite standard avec le TAEG et la duree
3. **Check du taux d'usure** : comparaison du TAEG avec le plafond 2e trimestre 2026 (5,19 pour cent sur 20 ans et plus, 4,48 pour cent sur 10 a 20 ans, 4 pour cent sur moins de 10 ans)

## Les limites du calcul

- Ne remplace pas une simulation bancaire officielle
- N'integre pas les frais de notaire (environ 7 a 8 pour cent du prix dans l'ancien, 2 a 3 pour cent dans le neuf)
- N'integre pas les frais de garantie (caution, hypotheque)
- N'integre pas les eventuels frais de courtage

## Strategies pour augmenter votre capacite d'emprunt

- **Solder les credits conso** : un credit de 300 euros par mois fait perdre 60 000 euros de capacite d'emprunt sur 20 ans
- **Deleguer l'assurance emprunteur** : 30 a 50 pour cent d'economie, TAEG plus bas
- **Augmenter l'apport** : reduit le capital a emprunter et rassure la banque
- **Allonger la duree** : augmente la capacite mais aussi le cout total

## Pour aller plus loin

- [Taux immobilier 2026 : faut-il acheter maintenant ?](/blog/taux-immobilier-2026/)
- [Taux d'usure 2e trimestre 2026 : les nouveaux plafonds](/blog/taux-usure-2e-trimestre-2026/)
- [Assurance emprunteur : delegation ou contrat groupe ?](/blog/assurance-emprunteur-delegation/)
- [Rachat de credit : quand est-ce rentable en 2026 ?](/blog/rachat-de-credit/)
- [Dispositif Jeanbrun : statut du bailleur prive](/blog/dispositif-jeanbrun-statut-bailleur-prive/)

<script>
(function() {
  var ids = ['revenu','charges','apport','duree','taux','assurance'];
  var nf = new Intl.NumberFormat('fr-FR', { style: 'currency', currency: 'EUR', maximumFractionDigits: 0 });
  var usureByDuration = { 10: 4.00, 15: 4.48, 20: 5.19, 25: 5.19 };

  function n(id) {
    return parseFloat(document.getElementById(id).value.replace(',', '.')) || 0;
  }

  function calc() {
    var revenu = n('revenu');
    var charges = n('charges');
    var apport = n('apport');
    var duree = parseInt(document.getElementById('duree').value, 10) || 20;
    var taux = n('taux');
    var assurance = n('assurance');

    var mensualiteMax = Math.max(0, revenu * 0.35 - charges);
    var taeg = taux + assurance;
    var r = taeg / 100 / 12;
    var nMonths = duree * 12;

    var capital = 0;
    if (mensualiteMax > 0 && r > 0) {
      capital = mensualiteMax * (1 - Math.pow(1 + r, -nMonths)) / r;
    } else if (r === 0) {
      capital = mensualiteMax * nMonths;
    }
    var budget = capital + apport;
    var interets = Math.max(0, mensualiteMax * nMonths - capital);

    document.getElementById('res-capital').textContent = nf.format(capital);
    document.getElementById('res-budget').textContent = nf.format(budget);
    document.getElementById('res-mensualite').textContent = nf.format(mensualiteMax);
    document.getElementById('res-interets').textContent = nf.format(interets);
    document.getElementById('res-taeg').textContent = taeg.toFixed(2).replace('.', ',') + ' %';

    var seuilUsure = usureByDuration[duree] || 5.19;
    var usureCard = document.getElementById('res-usure-card');
    var usureBadge = document.getElementById('res-usure');
    if (taeg <= seuilUsure) {
      usureBadge.textContent = 'Sous le plafond du taux d\u2019usure (' + seuilUsure.toFixed(2).replace('.', ',') + ' %)';
      usureCard.classList.remove('sim-alert');
      usureCard.classList.add('sim-ok');
    } else {
      usureBadge.textContent = 'Depasse le plafond du taux d\u2019usure (' + seuilUsure.toFixed(2).replace('.', ',') + ' %)';
      usureCard.classList.remove('sim-ok');
      usureCard.classList.add('sim-alert');
    }
  }

  ids.forEach(function(id) {
    var el = document.getElementById(id);
    if (el) el.addEventListener('input', calc);
  });
  calc();
})();
</script>
