<template>
  <v-app>
    <v-app-bar class="text-center" color="#121212" flat>
      <v-btn class="ma-0">
        <v-icon size="32">mdi-chevron-left</v-icon>
      </v-btn>

      <v-card-text style="font-size: 20px">All Orders</v-card-text>

      <v-btn disabled>
        <!--        <v-icon size="32">mdi-chevron-left</v-icon>-->
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>
