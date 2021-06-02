<template>
  <v-app>
    <v-system-bar color="deep-purple darken-3" collapse></v-system-bar>

    <v-app-bar dark prominent dense>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>
        <!--titulo navbar-->
      </v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-filter</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" absolute bottom temporary>
      <v-list nav dense>
        <v-list-item-group
          v-model="group"
          active-class="deep-purple--text text--accent-4"
        >
          <v-list-item>
            <v-list-item-title
              ><router-link to="/produto"
                >Produto</router-link
              ></v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-title
              ><router-link to="/produtos"
                >Produtos</router-link
              ></v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-title
              ><router-link to="/About">About</router-link></v-list-item-title
            >
          </v-list-item>

          <v-list-item>
            <v-list-item-title>-----</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <router-view></router-view>
    <v-footer color="primary lighten-1" padless>
        <v-row justify="center" no-gutters>
          <v-btn v-for="link in links" :key="link" color="white" text rounded class="my-2">
            {{ link }}
          </v-btn>
          <v-col class="primary lighten-2 py-4 text-center white--text" cols="12">
            {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
          </v-col>
        </v-row>
    </v-footer>
  </v-app>
</template>


<script>
export default {
  name: "App",
  data: () => ({
    drawer: false,
    group: null,
      links: [
        'Home',
        'About Us',
        'Team',
        'Services',
        'Blog',
        'Contact Us',
      ],
  }),

  watch: {
    group() {
      this.drawer = false;
    },
  },
};
</script>
