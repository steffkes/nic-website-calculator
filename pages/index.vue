<template>
  <section class="section">
    <div class="container is-max-desktop">
      <div class="columns">
        <div class="column">
          <table class="table is-fullwidth">
            <caption>
              Deine zukünftige Zeit- und Kostenbilanz nach unserer
              Zusammenarbeit
            </caption>
            <thead>
              <tr>
                <th style="text-align: left !important">Ersparnis</th>
                <th>Zeit</th>
                <th>Kosten</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <th>pro Woche</th>
                <td>{{ weeklySavedWorkingHours }} h</td>
                <td>{{ weeklySavedCosts }} €</td>
              </tr>
              <tr>
                <th>pro Monat</th>
                <td>{{ perMonth(weeklySavedWorkingHours) }} h</td>
                <td>{{ perMonth(weeklySavedCosts) }} €</td>
              </tr>
              <tr>
                <th>pro Jahr</th>
                <td>{{ perYear(weeklySavedWorkingHours) }} h</td>
                <td>{{ perYear(weeklySavedCosts) }} €</td>
              </tr>
            </tbody>
          </table>

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
                    placeholder="Wieviele Stunden arbeiten Sie pro Woche?"
                  />
                </div>
                <p class="control"><span class="button is-static">h</span></p>
              </div>
            </div>

            <div class="field">
              <label class="label">davon Büro-Anteil *</label>
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
        <div class="column">
          <div class="content">
            <div class="mb-3">
              <p
                v-if="weeklySavedWorkingHours >= 6"
                class="notification is-danger"
              >
                Du kannst noch einiges an Zeit und Kosten in deinem Betrieb
                einsparen! Buche dir jetzt einen Call mit mir und lass uns
                gemeinsam herausfinden, wie du wöchentlich bis zu
                {{ weeklySavedCosts }} € und
                {{ weeklySavedWorkingHours }} Stunden einsparen kannst!
              </p>

              <p
                v-else-if="weeklySavedWorkingHours >= 3"
                class="notification is-warning"
              >
                Da ist noch Potenzial, das du ausschöpfen kannst! Deine
                wöchentliche Einsparung liegt bei bis zu
                {{ weeklySavedCosts }} € Lass uns gerne darüber sprechen, wie
                ich dich noch weiter unterstützen kann.
              </p>

              <p v-else class="notification is-success">
                Du bist bereits gut aufgestellt, dennoch könnte für dich eine
                Ersparnis von bis zu {{ weeklySavedCosts }} € pro Woche möglich
                sein!
              </p>

              <div class="has-text-centered my-6">
                <p>
                  <a
                    :href="consultingLink"
                    target="_blank"
                    class="button is-primary is-fullwidth"
                    >Jetzt kostenfreies Kennenlerngespräch buchen</a
                  >
                </p>
              </div>
            </div>

            <div class="columns is-6">
              <div class="column">
                <table class="table is-fullwidth">
                  <caption>
                    Deine aktuelle Zeit- und Kostenbilanz
                  </caption>
                  <thead>
                    <tr>
                      <th>&nbsp;</th>
                      <th>Stunden</th>
                      <th>Kosten</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <th>pro Woche</th>
                      <td>{{ weeklyWorkingHours }} h</td>
                      <td>{{ weeklyCosts }} €</td>
                    </tr>

                    <tr>
                      <th>pro Monat</th>
                      <td>{{ perMonth(weeklyWorkingHours) }} h</td>
                      <td>{{ perMonth(weeklyCosts) }} €</td>
                    </tr>

                    <tr>
                      <th>pro Jahr</th>
                      <td>{{ perYear(weeklyWorkingHours) }} h</td>
                      <td>{{ perYear(weeklyCosts) }} €</td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <div class="column">
                <table class="table is-fullwidth">
                  <caption>
                    Deine zukünftige Zeit- und Kostenbilanz nach unserer
                    Zusammenarbeit
                  </caption>
                  <thead>
                    <tr>
                      <th style="text-align: left !important">Ersparnis</th>
                      <th>Zeit</th>
                      <th>Kosten</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <th>pro Woche</th>
                      <td>{{ weeklySavedWorkingHours }} h</td>
                      <td>{{ weeklySavedCosts }} €</td>
                    </tr>
                    <tr>
                      <th>pro Monat</th>
                      <td>{{ perMonth(weeklySavedWorkingHours) }} h</td>
                      <td>{{ perMonth(weeklySavedCosts) }} €</td>
                    </tr>
                    <tr>
                      <th>pro Jahr</th>
                      <td>{{ perYear(weeklySavedWorkingHours) }} h</td>
                      <td>{{ perYear(weeklySavedCosts) }} €</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>

            <p>
              Mit meiner Methode
              <strong
                >sparst du bis zu {{ perYear(weeklySavedWorkingHours) }} Stunden
                Zeit und {{ perYear(weeklySavedCosts) }} € pro Jahr</strong
              >
              – Zeit, die du gezielt für deine Familie, Freunde, Hobbys oder die
              strategische Weiterentwicklung deines Unternehmens nutzen kannst.
            </p>
            <p>
              Gleichzeitig reduzierst du unnötige Kosten und kannst das gesparte
              Geld in neue Maschinen, moderne Software oder die Weiterbildung
              deiner Mitarbeiter investieren.
              <strong
                >So machst du dein Unternehmen nicht nur effizienter, sondern
                auch zukunftssicherer</strong
              >.
            </p>

            <div class="has-text-centered my-6">
              <p>
                <a
                  :href="consultingLink"
                  target="_blank"
                  class="button is-primary is-fullwidth"
                  >Jetzt kostenfreies Kennenlerngespräch buchen</a
                >
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style>
:root {
  --bulma-primary-h: 45deg;
  --bulma-primary-s: 93%;
  --bulma-primary-l: 53%;
}
</style>

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
const {
  query: { name, email, phone },
} = useRoute();

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
  link.searchParams.set("name", name);
  link.searchParams.set("email", email);
  link.searchParams.set("a1", phone);
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
