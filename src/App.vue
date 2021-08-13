<template>
  <v-app>
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>
import Tarea from './components/Tarea.vue';

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
