<template>
  <q-layout>
    <q-toolbar slot="header" class="shadow-3">      
      <q-toolbar-title>
        TimeZoner App
        <span slot="subtitle">
          by Trinmar Boado
        </span>
      </q-toolbar-title>
      <!-- <q-search inverted color="none" v-model="word" @change="search(word)" /> -->
    </q-toolbar>
    <div class="layout-padding col" style="max-height: calc(100vh - 100px); overflow: auto">
      <!-- <pre>{{requests}}</pre> -->
      <div class="row wrap">
        <!-- <q-inline-datetime readonly v-model="dateNow" type="time" /> -->
        <q-inline-datetime style="height: 100%;" readonly v-model="dateNow" type="datetime" /> &nbsp;&nbsp; 
        <!-- <q-inline-datetime color="secondary" v-model="customDate" type="datetime" /> -->
        <q-card style="height: 100%;" v-for="(tz, i) in timeZones" :key="i">
          <q-card-title>
            <q-select
              @change="saveChanges"
              filter
              v-model="tz.timezone"
              float-label="Select timezone"
              :options="timezoneOptions"
            />
            <div slot="right">
              <q-toggle @change="saveChanges" v-model="tz.realtime" :label="tz.realtime ? 'Realtime' : 'Static'" />
            </div>
          </q-card-title>
          <q-card-main class="col">
            <q-input
              @change="saveChanges"
              v-model="tz.details"
              type="textarea"
              float-label="Details"
            />
            <q-inline-datetime v-if="tz.realtime" readonly color="secondary" :value="$moment(dateNow).tz(tz.timezone ? tz.timezone : 'America/New_York').format('YYYY-MM-DD HH:mm:ss')" type="datetime" />
            <q-inline-datetime v-if="!tz.realtime" readonly color="positive" :value="$moment(tz.datetime ? tz.datetime : $moment().format('YYYY-MM-DD HH:mm:ss') ).tz(tz.timezone ? tz.timezone : 'America/New_York').format('YYYY-MM-DD HH:mm:ss')" type="datetime" />
            <!-- <span >Items: asd</span> <br> -->
            <!-- <span class="text-faded"></span> -->
          </q-card-main>
          <q-card-separator />
          <q-card-actions class="col bg-light">
            <div class="col">
              <div class="row">
                <div class="col">
                  <q-field icon="today" v-if="!tz.realtime">
                    <q-datetime float-lab="Date and Time" @change="saveChanges" v-model="tz.datetime" type="datetime" />
                  </q-field>
                </div>
                <div class="row pull-right">
                  <q-btn @click="removeDate(i)" small round color="negative" icon="delete" class="animate-pop" />
                </div>
              </div>
            </div>
          </q-card-actions>
        </q-card>
      </div>
    </div>
    <q-fixed-position corner="bottom-right" :offset="[18, 18]">
      <!-- <q-btn @click="removeAll" round color="negative" icon="remove" class="animate-pop" /> -->
      <q-btn round color="primary" icon="today" class="animate-pop" @click="addNewDate" />
      <!-- <q-btn round color="primary" icon="date range" class="animate-pop" /> -->
      <!-- <q-btn @click="loadMore" round color="primary" icon="navigate next" class="animate-pop" /> -->
      <!-- <q-btn @click="loadAll" round color="primary" icon="last page" class="animate-pop" /> -->
    </q-fixed-position>

  </q-layout>
</template>

<script>
import { LocalStorage } from 'quasar'
import timezones from 'timezones'
let timezoneOptions = []
for (let i in timezones) {
  timezoneOptions.push({
    label: i,
    stamp: timezones[i],
    value: i
  })
}
export default {
  mounted () {
    setInterval(() => {
      this.dateNow = this.$moment().format('YYYY-MM-DD HH:mm:ss')
    }, 1000)

    this.timeZones = LocalStorage.get.item('timeZones') || []
  },
  data () {
    return {
      timezone: null,
      checked: true,
      dateNow: this.$moment().format('YYYY-MM-DD HH:mm:ss'),
      customDate: this.$moment().format('YYYY-MM-DD HH:mm:ss'),
      timezoneOptions,
      timeZones: [{
        datetime: '',
        timezone: '',
        realtime: true,
        details: ''
      }]
    }
  },
  methods: {
    addNewDate () {
      this.timeZones.unshift({
        datetime: '',
        timezone: '',
        realtime: true,
        details: ''
      })
      LocalStorage.set('timeZones', this.timeZones)
    },
    removeDate (i) {
      this.timeZones.splice(i, 1)
      LocalStorage.set('timeZones', this.timeZones)
    },
    saveChanges () {
      LocalStorage.set('timeZones', this.timeZones)
    }
  }
}
</script>
