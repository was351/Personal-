<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app clipped>
      <v-list>
        <v-list-item link v-for="menu in menus" :key="menu.name" :to="menu.url">
          <v-list-item-action>
            <v-icon>{{menu.icon}}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title class="text-uppercase">{{menu.name}}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-avatar>
            <v-chip v-if="menu.count > 0">{{menu.count}}</v-chip>
          </v-list-item-avatar>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />

      <v-toolbar-title>Anam Hira</v-toolbar-title>
      <v-spacer></v-spacer>

      <a href="https://github.com/anamhira47" target="_blank"><v-icon>mdi-github</v-icon></a>
      <a href="https://www.linkedin.com/in/anam-h-5b37a2125/" target="_blank"><v-icon>mdi-linkedin</v-icon></a>
      <a href="https://www.instagram.com/anamhira47/" target="_blank"><v-icon>mdi-instagram</v-icon></a>
      <a href="mailto:hira.anam49@gmail.com" target="_blank"><v-icon>mdi-email</v-icon></a>
      <v-avatar right>
        <img src="./assets/avatar.jpg" />
      </v-avatar>
    </v-app-bar>

    <v-main>
      <v-container class="fill-height" fluid>
        <router-view></router-view>
      </v-container>
    </v-main>

    <v-footer app>
      <span>Anam Hira &copy; 2020</span>
    </v-footer>
  </v-app>
</template>

<style lang="scss">
.line-break {
  white-space: pre-wrap;
}

 a:-webkit-any-link{
   text-decoration:none !important;
 }

</style>

<script>
import Axios from "axios";

export default {
  props: {
    source: String
  },
  data: () => ({
    drawer: null,
    menus: [
      {
        name: "cv",
        url: "/cv",
        icon: "mdi-file-document-box-multiple-outline",
        count: 0
      },

      {
        name: "researches",
        url: "/researches",
        icon: "mdi-school-outline",
        count: 0
      },
      {
        name: "projects",
        url: "/projects",
        icon: "mdi-briefcase-outline",
        count: 0
      },
      {
        name: "Resume",
        url: "/resume",
        icon: "mdi-cv",
        count: 0
      }
    ],
    cv: {}
  }),
  created() {
    this.$vuetify.theme.dark = true;
  },
  mounted: function() {
    this.getCount();
    this.getProjectCount();
  },
  methods: {
    getCount: function() {
      var _this = this;
      Axios.get("/data/cv.json").then(function(response) {
        _this.cv = response.data;
        _this.menus.forEach(function(menu) {
          if (_this.cv[menu.name] !== undefined) {
            menu.count = _this.cv[menu.name].length;
          }
        });
      });
    },
    getProjectCount: function() {
      var _this = this;
      Axios.get("/data/projects.json").then(function(response) {
        // /* eslint-disable no-console */
        // console.log(response.data.length);
        _this.menus.forEach(function(menu) {
          if (menu.name === "projects") {
            response.data.projects.forEach(function(item) {
              menu.count += item.repos.length;
            });
          }
        });
      });
    }
  }
};
</script>
