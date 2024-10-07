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

<p><strong>Potenzial: {{ Math.round((20/100) * (share / 100) * hours) }}h (20% von {{ (share / 100) * hours }}h, gerundet)</strong></p>

<p class="my-3"><a :href="consultingLink">Jetzt Beratungsgespräch buchen</a></p>

<p>Wochenumsatz = {{ rate * hours }}€ ({{ rate }}€ × {{ hours }}h)</p>
<p>Büroarbeit = {{ (share / 100) * hours }}h ({{ share }}% von {{ hours }}h)</p>


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
</script>
