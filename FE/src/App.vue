<template>
  <v-app>
    <v-app-bar
      app
      color="#FFFFFF"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt=" Logo"
          class=""
          contain
          :src="images.philti"
          transition="scale-transition"
          width="150"
        />
      </div>

      <v-spacer></v-spacer>
      <h1 class="text-h4  black--text transition-swing">Vykrytie špičky</h1>
      <!-- <div
              class="text-h4  black--text transition-swing"
            >
        Vykritie špičky
      </div> -->

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/MainForm';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    images: {
        philti: require('./assets/philti.png')
    }
  }),
};
</script>
