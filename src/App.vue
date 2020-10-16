<template>
  <v-app>
    <v-navigation-drawer v-model="isDrawerOpen" app>
      <!-- -->
    </v-navigation-drawer>

    <v-app-bar color="white" app elevation="0">
      <v-app-bar-nav-icon
        @click="isDrawerOpen = !isDrawerOpen"
      ></v-app-bar-nav-icon>
      <div class="d-flex">
        <v-icon>mdi-calendar</v-icon>
        <span class="pl-2">
          <span style="color: purple">Bowskie's</span> Calendar</span
        >
      </div>
      <v-row class="d-flex justify-space-between">
        <div class="d-flex align-center pl-12">
          <v-btn class="mr-2" outlined small @click="setToday()">Today</v-btn>
          <v-btn class="mr-2" outlined small @click="addClass()"
            >Add zajecie xd</v-btn
          >

          <div class="pr-2">
            <v-btn icon small @click="prev()"
              ><v-icon small>mdi-chevron-left</v-icon></v-btn
            >
            <v-btn icon small @click="next()"
              ><v-icon small>mdi-chevron-right</v-icon></v-btn
            >
          </div>
          <span v-if="$refs.calendar">{{ $refs.calendar.title }}</span>
        </div>

        <div>
          <!-- <span v-if="$refs.calendar">{{ $refs.calendar.end }}</span> -->
          <v-btn icon small
            ><v-icon small>mdi-help-circle-outline</v-icon></v-btn
          >
          <v-menu bottom right>
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                class="ml-3"
                small
                outlined
                color="grey darken-2"
                v-bind="attrs"
                v-on="on"
              >
                <span>{{ typeToLabel[type] }}</span>
                <v-icon right> mdi-menu-down </v-icon>
              </v-btn>
            </template>
            <v-list>
              <v-list-item @click="type = 'day'">
                <v-list-item-title>Day</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = 'week'">
                <v-list-item-title>Week</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = 'month'">
                <v-list-item-title>Month</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = '4day'">
                <v-list-item-title>4 days</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
          <v-avatar class="ml-4" size="32" color="purple"></v-avatar>
        </div>
      </v-row>
    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>
      <!-- Provides the application the proper gutter -->

      <v-sheet height="100%">
        <v-calendar
          :events="events"
          ref="calendar"
          v-model="focus"
          :type="type"
          end="2021-06-21"
        ></v-calendar>
      </v-sheet>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "App",

  components: {},

  data: () => ({
    isDrawerOpen: false,
    focus: "",
    type: "month",
    typeToLabel: {
      month: "Month",
      week: "Week",
      day: "Day",
      "4day": "4 Days",
    },
    events: [],

    //
  }),
  methods: {
    addClass() {
      const data = {
        title: "ZajeciaTestowe",
        description: "To opis",
        day: 1,
        hour: 13,
        minute: 30,
        additionalInfo: "Dodatkowe Info",
      };
      axios
        .post("http://localhost:3001/zajecia", data)
        .then((res) => {
          for (let i of res.data) {
            let tempDate = new Date(new Date(i.date).getTime());
            this.events.push({
              start: new Date(i.date)
                .toISOString()
                .substring(0, 16)
                .replace("T", " "),
              end: new Date(tempDate.setTime(tempDate.getTime() + 5400000))
                .toISOString()
                .substring(0, 16)
                .replace("T", " "),
              name: i.title,
            });
          }
        })
        .catch((err) => console.log(err));
    },
    prev() {
      this.$refs.calendar.prev();
    },
    next() {
      this.$refs.calendar.next();
    },
    setToday() {
      this.focus = "";
    },
  },
};
</script>
