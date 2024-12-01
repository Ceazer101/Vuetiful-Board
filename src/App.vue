<template>
  <v-app id="inspire">
    <v-navigation-drawer
        v-model="drawer"
        image="https://cdn.vuetifyjs.com/images/backgrounds/bg-2.jpg"
        theme="dark"
        permanent
      >
      <v-sheet
        class="pa-5"
        
      >
        <div>
          <v-icon left>mdi-account</v-icon>
          john@google.com
        </div>
      </v-sheet>

      <template v-slot:image>
        <v-img
          gradient="to top right, rgba(19,84,122,.8), rgba(128,208,199,.8)"
        ></v-img>
      </template>

        <v-list nav>
          <v-list-item prepend-icon="mdi-folder" title="Boards" value="boards"></v-list-item>
          <v-list-item prepend-icon="mdi-share-variant" title="Shared" value="shared"></v-list-item>
          <router-link to="/about">
            <v-list-item prepend-icon="mdi-information" title="About" value="about"></v-list-item>
          </router-link>
        </v-list>
      </v-navigation-drawer>

    <v-app-bar
        color="teal-darken-4"
        image="https://cdn.vuetifyjs.com/images/backgrounds/bg-2.jpg"
      >
        <template v-slot:image>
          <v-img
            gradient="to top left, rgba(19,84,122,.8), rgba(128,208,199,.8)"
          ></v-img>
        </template>

        <template v-slot:prepend>
          <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
        </template>

        <v-app-bar-title>Vuetiful Board</v-app-bar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <router-link to="/" exact>
            <v-icon>mdi-home</v-icon>
          </router-link>
        </v-btn>
      </v-app-bar>

    <v-main>
      <RouterView />
    </v-main>
  </v-app>
</template>

<script>
import { RouterView } from 'vue-router';

  export default {
    
    data: () => ({ drawer: null }),
  }
</script>