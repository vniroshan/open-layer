<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center text-h5">
      Open layers Map
      </div>

      <v-spacer></v-spacer>

      <v-btn
        text
      >
        <span class="mr-2"> NIROSHAN SE-2017-031</span>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
