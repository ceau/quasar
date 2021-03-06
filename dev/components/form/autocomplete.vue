<template>
  <div>
    <div class="layout-padding" style="max-width: 600px;">
      <p class="caption" style="margin-bottom: 40px">
        These examples feature countries autocomplete.<br>
        On desktop, Escape key closes the suggestions popover and you can navigate with keyboard arrow keys. Selection is made with either mouse/finger tap or by Enter key.
      </p>


      <q-search v-model="terms" placeholder="Start typing a country name">
        <q-popover fit>
          <div style="padding: 10px" class="bg-primary text-white">
            Some Popover
          </div>
        </q-popover>
      </q-search>

      <q-search v-model="terms" placeholder="Start typing a country name">
        <q-autocomplete v-model="terms" @search="search" @selected="selected" :delay="0" />
      </q-search>

      <br>

      <p class="caption">Maximum of 2 results at a time</p>
      <q-search v-model="terms">
        <q-autocomplete
          v-model="terms"
          @search="search"
          :max-results="2"
          @selected="selected"
          :delay="0"
        />
      </q-search>

      <br>

      <p class="caption">Minimum 3 characters to trigger search</p>
      <q-input v-model="terms" placeholder="Type 'fre'">
        <q-autocomplete
          v-model="terms"
          @search="search"
          :min-characters="3"
          @selected="selected"
        />
      </q-input>

      <br>

      <p class="caption">Static List</p>
      <q-search v-model="terms" placeholder="Featuring static data">
        <q-autocomplete
          v-model="terms"
          :static-data="{field: 'value', list: countries}"
          @selected="selected"
          :delay="0"
        />
      </q-search>

      <br>

      <p class="caption">Delimiter between results</p>
      <q-search v-model="terms">
        <q-autocomplete
          v-model="terms"
          delimiter
          @search="search"
          @selected="selected"
          :delay="0"
        />
      </q-search>
    </div>
  </div>
</template>

<script>
import { uid, filter, Toast } from 'quasar'
import countries from 'data/autocomplete.json'

const icons = ['alarm', 'email', 'search', 'build', 'card_giftcard', 'perm_identity', 'receipt', 'schedule', 'speaker_phone', 'archive', 'weekend', 'battery_charging_full']

function getRandomIcon () {
  return icons[Math.floor(Math.random() * icons.length)]
}
function getRandomStamp () {
  if (Math.floor(Math.random() * 50) % 3 === 0) {
    return `${Math.floor(Math.random() * 10)} min`
  }
}
function getRandomSecondLabel () {
  if (Math.floor(Math.random() * 50) % 4 === 0) {
    return `UID: ${uid().substring(0, 8)}`
  }
}
function parseCountries () {
  return countries.map(country => {
    return {
      label: country,
      secondLabel: getRandomSecondLabel(),
      icon: getRandomIcon(),
      stamp: getRandomStamp(),
      value: country
    }
  })
}

export default {
  data () {
    return {
      terms: '',
      countries: parseCountries()
    }
  },
  methods: {
    search (terms, done) {
      setTimeout(() => {
        done(filter(terms, {field: 'value', list: parseCountries()}))
      }, 500)
    },
    selected (item) {
      Toast.create(`Selected suggestion "${item.label}"`)
    }
  }
}
</script>
