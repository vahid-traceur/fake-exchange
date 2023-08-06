<template>
  <v-app>
    <v-app-bar class="text-center" color="#121212" flat>
      <v-btn class="ma-0">
        <v-icon size="32">mdi-chevron-left</v-icon>
      </v-btn>

      <v-toolbar-title>All Orders</v-toolbar-title>

      <v-btn disabled>
        <!--        <v-icon size="32">mdi-chevron-left</v-icon>-->
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>
