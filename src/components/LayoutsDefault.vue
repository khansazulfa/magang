<template>
  <v-card
    height="100vh"
    width="256"
  >
    <v-navigation-drawer permanent class="pa-8">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            Application
          </v-list-item-title>
          <v-list-item-subtitle>
            subtext
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <v-list style="position: absolute; bottom: 20px">
        <v-list-item>
          <v-btn @click="logout()" :loading="loading" class="text-capitalize" color="error">
            <v-icon class="mr-2">mdi-power</v-icon>
            Logout
          </v-btn>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-alert v-if="alert" type="success">
      Berhasil logout
    </v-alert>
  </v-card>
</template>

<script>
  export default {
    data () {
      return {
        items: [
          { title: 'Dashboard', icon: 'mdi-view-dashboard' },
          { title: 'Photos', icon: 'mdi-image' },
          { title: 'About', icon: 'mdi-help-box' },
        ],
        right: null,
        alert: false,
        loading:  false,
      }
    },
    created() {
      const user = localStorage.getItem('user');
      if(!user || localStorage.length < 1) return this.$router.push('/login');
    },
    methods: {
      logout() {
        this.loading = true;
        this.alert = true;
        setTimeout(() => {
          this.alert = false;
          this.loading = false
          localStorage.clear();
          window.location.reload();
        }, 2000);
      }
    }
  }
</script>