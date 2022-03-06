<template>
  <!-- App.vue -->
  <v-app>
    <Alert />
    <Dialog />
  
    <v-navigation-drawer app v-model="drawer" color="teal lighten-5">
      <v-list>
        <v-list-item v-if="!guest">
          <v-list-item-avatar>
            <v-img>
              <img
                :src="
                  user.photo_profile
                    ? apiDomain + user.photo_profile
                    : 'https://randomuser.me/api/portraits/men/75.jpg'"/></v-img>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title>{{ user.name }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <div class="pa-2" v-if="guest">
          <v-btn dark color="teal darken-1" class="mb-1" @click="login">
            <v-icon left>mdi-lock</v-icon>
            Login
          </v-btn>
          <v-btn dark color="teal lighten-2" @click="regis">
            <v-icon left>mdi-account</v-icon>
            Registrasi
          </v-btn>
        </div>

        <v-divider></v-divider>

        <v-list-item
          v-for="(item, index) in menus"
          :key="`menu-${index}`"
          :to="item.route"
        >
          <v-list-item-icon>
            <v-icon left class="teal--text">{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title class="teal--text">{{
              item.title
            }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>

      <template v-slot:append v-if="!guest">
        <div class="pa-2">
          <v-btn block color="teal" dark @click="logout">
            <v-icon left>mdi-lock</v-icon>
            Logout
          </v-btn>
        </div>
      </template>
    </v-navigation-drawer>

    <v-app-bar app color="teal darken-4" dark>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>VueJs Batch 32 Final Project</v-toolbar-title>

      <!-- Pemisah konten -->
      <v-spacer></v-spacer>
    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>
      <!-- Provides the application the proper gutter -->
      <v-container fluid>
        <v-slide-y-transition>
          <!-- If using vue-router -->
          <router-view></router-view>
        </v-slide-y-transition>
      </v-container>
    </v-main>

    <v-footer
    dark
    padless>
    <v-card
      class="flex"
      flat
      tile>
      <v-card-title class="teal text-">
        <strong class="subheading">M Regina - Reza Berlyva</strong>
        <v-spacer></v-spacer>
      </v-card-title>

      <v-card-text class="py-2 white--text text-center">
        {{ new Date().getFullYear() }} — <strong>Sanbercode VueJS Batch 32</strong>
      </v-card-text>
    </v-card>
  </v-footer>
  </v-app>
</template>

<script>
import { mapActions, mapGetters } from "vuex";
import Alert from "./components/AlertStore.vue";
import Dialog from "./components/DialogStore.vue";
export default {
  components: { Alert, Dialog },
  name: "App",
  data: () => ({
    drawer: false,
    menus: [
      { title: "Home", icon: "mdi-home", route: "/" },
      { title: "Blogs", icon: "mdi-note", route: "/blogs" },
    ],
    apiDomain: "https://demo-api-vue.sanbercloud.com",
  }),
  computed: {
    ...mapGetters({
      guest: "auth/guest",
      user: "auth/user",
      token: "auth/token",
    }),
  },
  methods: {
    logout() {
      let config = {
        method: "post",
        url: this.apiDomain + "/api/v2/auth/logout",
        headers: {
          Authorization: "Bearer " + this.token,
        },
      };
      this.axios(config)
        .then(() => {
          this.setToken("");
          this.setUser({});
          this.setAlert({
            status: true,
            color: "success",
            text: "Anda berhasil Logout",
          });
        })
        .catch(() => {
          this.setAlert({
            status: true,
            color: "error",
            text: "Anda gagal Logout",
          });
        });
    },
    login() {
      this.setDialogComponent({ component: "login" });
    },
    regis() {
      this.setDialogComponent({ component: "regis" });
    },
    ...mapActions({
      setAlert: "alert/set",
      setDialogComponent: "dialog/setComponent",
      setUser: "auth/setUser",
      setToken: "auth/setToken",
      checkToken: "auth/checkToken",
    }),
  },
  mounted() {
    if (this.token) {
      this.checkToken(this.token);
    }
  },
};
</script>