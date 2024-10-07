<template>
  <section class="section">
    <div class="container">

<div class="columns">

<div class="column">

<form @submit="handleProgress">

<div class="field">
  <label class="label">Name *</label>
  <div class="control">
    <input type="text" v-model="name" class="input" required="true" placeholder="Ihr Name">
  </div>
</div>

<div class="field">
  <label class="label">Firma *</label>
  <div class="control">
    <input type="text" v-model="company" class="input" required="true" placeholder="Name der Firma">
  </div>
</div>

<div class="field">
  <label class="label">eMail *</label>
  <div class="control">
    <input type="email" v-model="email" class="input" required="true" placeholder="Ihre eMail-Adresse">
  </div>
</div>

<div class="field">
  <label class="label">Telefon</label>
  <div class="control">
    <input type="text" v-model="phone" class="input" placeholder="Ihre Telefon-Nummer">
  </div>
</div>

<div class="field is-grouped is-grouped-right">
  <div class="control">
    <button type="submit" class="button is-primary">Weiter</button>
  </div>
</div>

</form>

</div>
<div class="column">
<form :class="{'is-hidden': step < 1}" @submit="handleProgress">

<div class="field">
  <label class="label">Stundensatz *</label>
  <div class="field has-addons">
  <div class="control">
    <input type="number" v-model="rate" class="input" required="true" placeholder="Wieviel verrechnen Sie pro Stunde?">
  </div>
  <p class="control"><span class="button is-static">€</span></p>
  </div>
</div>

<div class="field">
  <label class="label">Wochenarbeitsstunden *</label>
  <div class="control">
    <input type="number" v-model="hours" class="input" required="true" placeholder="Wieviele Stunden arbeiten Sie pro Woche?">
  </div>
</div>

<div class="field">
  <label class="label">Büro-Anteil *</label>
  <div class="field has-addons">
  <div class="control">
    <input type="number" v-model="share" class="input" required="true" placeholder="Wieviel davon verbringen Sie im Büro?">
  </div>
  <p class="control"><span class="button is-static">%</span></p>
  </div>
</div>

<div class="field is-grouped is-grouped-right">
  <div class="control">
    <button type="submit" class="button is-primary">Weiter</button>
  </div>
</div>

</form>
</div>
<div class="column">
<div :class="{'is-hidden': step < 2}">

<p class="mb-3"><a :href="consultingLink">Jetzt Beratungsgespräch buchen</a></p>

<table>
<caption>Deine aktuelle Zeit- und Kostenbilanz</caption>
<thead>
<tr>
<th>&nbsp;</th>
<th>pro Woche</th>
<th>pro Monat</th>
<th>pro Jahr</th>
</tr>
</thead>
<tbody>
<tr>
<th>Stunden</th>
<td>{{ weeklyWorkingHours }}</td>
<td>{{ perMonth(weeklyWorkingHours) }}</td>
<td>{{ perYear(weeklyWorkingHours) }}</td>
</tr>
<tr>
<th>Kosten</th>
<td>{{ weeklyCosts }}</td>
<td>{{ perMonth(weeklyCosts ) }}</td>
<td>{{ perYear(weeklyCosts ) }}</td>
</tr>
</tbody>
</table>

<table>
<caption>Deine mögliche Zukunft nach unserer Zusammenarbeit</caption>
<thead>
<tr>
<th>&nbsp;</th>
<th>pro Woche</th>
<th>pro Monat</th>
<th>pro Jahr</th>
</tr>
</thead>
<tbody>
<tr>
<th>Zeitersparnis</th>
<td>{{ weeklySavedWorkingHours }}</td>
<td>{{ perMonth(weeklySavedWorkingHours) }}</td>
<td>{{ perYear(weeklySavedWorkingHours) }}</td>
</tr>
<tr>
<th>Kostenersparnis</th>
<td>{{ weeklySavedCosts }}</td>
<td>{{ perMonth(weeklySavedCosts ) }}</td>
<td>{{ perYear(weeklySavedCosts ) }}</td>
</tr>
</tbody>
</table>

<p>Mit meiner Methode kannst du bis zu {{ perYear(weeklySavedWorkingHours) }}
Stunden im Jahr mehr Zeit haben! Zum Beispiel für deine Familie, Freunde,
Hobbies, neue Kunden oder strategische Planung, um dein Unternehmen noch
erfolgreicher zu machen.</p>

<p>Mit meiner Methode kannst du bis zu {{ perYear(weeklySavedCosts ) }} € im
Jahr einsparen, die du zum Beispiel in neue Maschinen, Software, in die
Weiterbildung deiner Mitarbeiter oder in deine Altersvorsorge, investieren
kannst.</p>

</div>
</div>
</div>


<div class="has-text-grey-lighter" style="border: 1px solid #f0f0f0;">

<p>Step = {{ step }}</p>
<p>Kontakt:
Name = {{ name }} |
Company = {{ company }} |
Mail = {{ email }} |
Phone = {{ phone }}
</p>
<p>Daten:
Rate = {{ rate }} |
Hours = {{ hours }} |
Share = {{ share }}
</p>

</div>

    </div>
  </section>
</template>

<script setup>
const step = ref(0);

const name = ref("");
const company = ref("");
const email = ref("");
const phone = ref("");

const rate = ref("");
const hours = ref("");
const share = ref("");

const handleProgress = (event) => {
console.debug({ event })
event.preventDefault();
event.stopPropagation();
step.value = step.value + 1;
}

const consultingLink = computed( () => {

const link = new URL("https://calendly.com/nicolasfaure/60");
link.searchParams.set("name", name.value);
link.searchParams.set("email", email.value);
link.searchParams.set("a1", phone.value);
return link.href;
});

const weeklyWorkingHours = computed( () => Math.round((share.value / 100) * hours.value) );
const weeklyCosts = computed( () => rate.value * weeklyWorkingHours.value );

const weeklySavedWorkingHours = computed( () => Math.round((20/100) * (share.value / 100) * hours.value) );
const weeklySavedCosts = computed( () => rate.value * weeklySavedWorkingHours.value );

const perMonth = (value) => value * 4;
const perYear = (value) => perMonth(value) * 12;

</script>
