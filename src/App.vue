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
          <v-btn @click="testData()">TESTESTETSTE</v-btn>
          <v-btn class="mr-2" outlined small @click="setToday()">Today</v-btn>

          <v-row justify="center">
            <v-dialog v-model="dialog" max-width="400px">
              <template v-slot:activator="{ on }">
                <v-btn v-on="on" class="mr-4 ml-4" outlined small
                  >Add zajecie xd</v-btn
                >
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">Dodaj cykliczne zajęcia</span>
                </v-card-title>
                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field
                          required
                          label="Nazwa zajęć"
                          v-model="classTitle"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-text-field
                          v-model="classLeader"
                          label="Prowadzący"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-text-field
                          v-model="classDay"
                          label="Dzien tygodnia"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-menu
                          ref="menu"
                          :close-on-content-click="false"
                          :nudge-right="40"
                          :return-value.sync="classTime"
                          transition="scale-transition"
                          offset-y
                          max-width="290px"
                          min-width="290px"
                        >
                          <template v-slot:activator="{ on, attrs }">
                            <v-text-field
                              v-model="classTime"
                              v-on="on"
                              required
                              label="Godzina rozpoczęcia zajęć"
                              prepend-icon="mdi-clock-time-four-outline"
                              readonly
                              v-bind="attrs"
                            ></v-text-field>
                          </template>
                          <v-time-picker
                            @click:minute="$refs.menu.save(classTime)"
                            v-model="classTime"
                          ></v-time-picker>
                        </v-menu>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn @click="submitClass()" text>zapisz</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-row>

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
    classDay: 0,
    classTitle: "",
    classTime: null,
    classLeader: "",
    isDrawerOpen: false,
    focus: "",
    dialog: false,
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
    submitClass() {
      const dataToSend = {
        title: this.classTitle,
        hour: this.classTime.substring(0, 2),
        minute: this.classTime.substring(3, 5),
        description: this.classLeader,
        day: this.classDay * 1,
        additionalInfo: "siema",
      };
      console.log(dataToSend);
      axios
        .post("http://localhost:3001/zajecia", dataToSend)
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
    testData() {
      console.log(this.classTime.substring(3, 5));
      console.log(this.classLeader);
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
