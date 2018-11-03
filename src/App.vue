<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      fixed
      app
    >
      <v-list dense>
        
        <v-list-tile to="/">
          <v-list-tile-action>
            <v-icon>home</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>Home</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile to="/pxtopt">
          <v-list-tile-action>
            <v-icon>save_alt</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>PXToPT</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile to="/sendEmail">
          <v-list-tile-action>
            <v-icon>email</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>SendEmail</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

        <v-list-tile to="/textdiff">
          <v-list-tile-action>
            <v-icon>compare</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>TextDiff</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

      </v-list>
    </v-navigation-drawer>
    <v-toolbar color="indigo" dark fixed app>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>Application</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <router-view/>
    </v-content>
    <v-footer color="indigo" app>
      <span class="white--text">&copy; 2018</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      drawer: null
    }),
    props: {
      source: String
    }
  }
</script>
