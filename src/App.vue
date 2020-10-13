<template>
  <v-app>
    <v-navigation-drawer v-model="isDrawerOpen" app>
      <!-- -->
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon
        @click="isDrawerOpen = !isDrawerOpen"
      ></v-app-bar-nav-icon>
      <div class="d-flex">
        <v-icon>mdi-calendar</v-icon>
        <span class="pl-2">Kalendarz</span>
      </div>
      <v-row class="d-flex justify-space-between">
        <div class="d-flex align-center pl-12">
          <v-btn class="mr-2" outlined small>Dzisiaj</v-btn>
          <div class="pr-2">
            <v-btn icon small><v-icon small>mdi-chevron-left</v-icon></v-btn>
            <v-btn icon small><v-icon small>mdi-chevron-right</v-icon></v-btn>
          </div>
          <span>Październik 2020</span>
        </div>

        <div>
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
                <span>Month</span>
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
      <v-container fluid> </v-container>
    </v-main>

    <v-footer app>
      <!-- -->
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: () => ({
    isDrawerOpen: false,
    //
  }),
};
</script>
