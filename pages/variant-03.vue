<template>
  <section class="section">
    <div class="container is-max-desktop">
      <div class="columns">
        <div class="column">
          <table class="table is-fullwidth">
            <caption>
              Deine mögliche Zeit- & Kostenersparnis
            </caption>
            <thead>
              <tr>
                <th>&nbsp;</th>
                <th colspan="3">Stunden / Kosten</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <th rowspan="2">pro Woche</th>
                <td>{{ weeklyWorkingHours }} h</td>
                <td>- {{ weeklySavedWorkingHours }} h</td>
                <td>= {{ weeklyWorkingHours - weeklySavedWorkingHours }} h</td>
              </tr>
              <tr>
                <td>{{ weeklyCosts }} €</td>
                <td>- {{ weeklySavedCosts }} €</td>
                <td>= {{ weeklyCosts - weeklySavedCosts }} €</td>
              </tr>

              <tr>
                <th rowspan="2">pro Monat</th>
                <td>{{ perMonth(weeklyWorkingHours) }} h</td>
                <td>- {{ perMonth(weeklySavedWorkingHours) }} h</td>
                <td>
                  =
                  {{ perMonth(weeklyWorkingHours - weeklySavedWorkingHours) }}
                  h
                </td>
              </tr>
              <tr>
                <td>{{ perMonth(weeklyCosts) }} €</td>
                <td>- {{ perMonth(weeklySavedCosts) }} €</td>
                <td>= {{ perMonth(weeklyCosts - weeklySavedCosts) }} €</td>
              </tr>

              <tr>
                <th rowspan="2">pro Jahr</th>
                <td>{{ perYear(weeklyWorkingHours) }} h</td>
                <td>- {{ perYear(weeklySavedWorkingHours) }} h</td>
                <td>
                  =
                  {{ perYear(weeklyWorkingHours - weeklySavedWorkingHours) }}
                  h
                </td>
              </tr>
              <tr>
                <td>{{ perYear(weeklyCosts) }} €</td>
                <td>- {{ perYear(weeklySavedCosts) }} €</td>
                <td>= {{ perYear(weeklyCosts - weeklySavedCosts) }} €</td>
              </tr>
            </tbody>
          </table>

          <div class="has-text-centered my-4">
            <p>
              <a :href="consultingLink" class="button is-primary"
                >Jetzt Beratungsgespräch buchen</a
              >
            </p>
          </div>
        </div>
        <div class="column">
          <form @submit="handleProgress">
            <div class="field">
              <label class="label">Stundensatz *</label>
              <div class="field has-addons">
                <div class="control">
                  <input
                    type="number"
                    v-model="rate"
                    class="input"
                    required="true"
                    placeholder="Wieviel verrechnen Sie pro Stunde?"
                  />
                </div>
                <p class="control"><span class="button is-static">€</span></p>
              </div>
            </div>

            <div class="field">
              <label class="label">Wochenarbeitsstunden *</label>
              <div class="field has-addons">
                <div class="control">
                  <input
                    type="number"
                    v-model="hours"
                    class="input"
                    required="true"
                    placeholder="Wieviel davon verbringen Sie im Büro?"
                  />
                </div>
                <p class="control"><span class="button is-static">h</span></p>
              </div>
            </div>

            <div class="field">
              <label class="label">Büro-Anteil *</label>
              <div class="field has-addons">
                <div class="control">
                  <input
                    type="number"
                    v-model="share"
                    class="input"
                    required="true"
                    placeholder="Wieviel davon verbringen Sie im Büro?"
                  />
                </div>
                <p class="control"><span class="button is-static">%</span></p>
              </div>
            </div>
          </form>
        </div>
      </div>

      <div class="content">
        <div class="mb-3">
          <p v-if="weeklySavedWorkingHours >= 6" class="notification is-danger">
            Du kannst noch einiges an Zeit und Kosten in deinem Betrieb
            einsparen! Buche dir jetzt einen Call mit mir und lass uns gemeinsam
            herausfinden, wie du wöchentlich bis zu
            {{ weeklySavedCosts }} € und {{ weeklySavedWorkingHours }} Stunden
            einsparen kannst!
          </p>

          <p
            v-else-if="weeklySavedWorkingHours >= 3"
            class="notification is-warning"
          >
            Da ist noch Potenzial, das du ausschöpfen kannst! Deine wöchentliche
            Einsparung liegt bei bis zu
            {{ weeklySavedCosts }} € Lass uns gerne darüber sprechen, wie ich
            dich noch weiter unterstützen kann.
          </p>

          <p v-else class="notification is-success">
            Herzlichen Glückwunsch! Du bist richtig gut aufgestellt: Deine
            Ersparnis pro Woche liegt bei bis zu {{ weeklySavedCosts }} €
          </p>
        </div>

        <p>
          Mit meiner Methode kannst du bis zu
          {{ perYear(weeklySavedWorkingHours) }} Stunden im Jahr mehr Zeit
          haben! Zum Beispiel für deine Familie, Freunde, Hobbies, neue Kunden
          oder strategische Planung, um dein Unternehmen noch erfolgreicher zu
          machen.
        </p>

        <p>
          Mit meiner Methode kannst du bis zu
          {{ perYear(weeklySavedCosts) }} € im Jahr einsparen, die du zum
          Beispiel in neue Maschinen, Software, in die Weiterbildung deiner
          Mitarbeiter oder in deine Altersvorsorge, investieren kannst.
        </p>

        <div class="has-text-centered my-4">
          <p>
            <a :href="consultingLink" class="button is-primary"
              >Jetzt Beratungsgespräch buchen</a
            >
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
table caption {
  text-align: left;
}

table tbody td,
table thead th {
  text-align: right !important;
}

table tbody td {
  font-variant-numeric: tabular-nums;
  text-wrap: nowrap;
}
</style>

<script setup>
const rate = ref(60);
const hours = ref(40);
const share = ref(20);

const handleProgress = (event) => {
  console.debug({ event });
  event.preventDefault();
  event.stopPropagation();
};

const consultingLink = computed(() => {
  const link = new URL("https://calendly.com/nicolasfaure/60");
  return link.href;
});

const weeklyWorkingHours = computed(() =>
  Math.round((share.value / 100) * hours.value),
);
const weeklyCosts = computed(() => rate.value * weeklyWorkingHours.value);

const weeklySavedWorkingHours = computed(() =>
  Math.round((20 / 100) * (share.value / 100) * hours.value),
);
const weeklySavedCosts = computed(
  () => rate.value * weeklySavedWorkingHours.value,
);

const perMonth = (value) => value * 4;
const perYear = (value) => perMonth(value) * 12;
</script>
