<template>
  <v-app>
    <div v-if="!$router.currentRoute.meta.plainLayout" class="d-flex align-start">
      <layouts-default />
      <v-main>
        <router-view />
      </v-main>
    </div>

    <div v-if="$router.currentRoute.meta.plainLayout">
      <v-main>
        <router-view />
      </v-main>
    </div>
  </v-app>
</template>

<script>
import LayoutsDefault from '../src/components/LayoutsDefault.vue'
export default {
  name: 'App',
  components: {
    LayoutsDefault
  },
  data() {
    return {

    }
  },
  mounted() {
    console.log('testing route', this.$router);
  }
};
</script>
