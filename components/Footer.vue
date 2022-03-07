<template>
  <v-footer app color="rgba(107,78,113,.50)">
    <v-btn fab outlined small color="secondary" elevation="11" @click="$vuetify.goTo(0)">
      <v-icon color="secondary" @click="$vuetify.goTo(0)">
        mdi-chevron-up
      </v-icon>
    </v-btn>
    <v-btn fab outlined small color="secondary" elevation="11" @click="$vuetify.goTo(9999)">
      <v-icon color="secondary" @click="$vuetify.goTo(9999)">
        mdi-chevron-down
      </v-icon>
    </v-btn>
    <v-spacer />
    <v-btn fab color="accent" elevation="11" to="/bookInput">
      <v-icon color="primary">
        mdi-plus
      </v-icon>
    </v-btn>
  </v-footer>
</template>