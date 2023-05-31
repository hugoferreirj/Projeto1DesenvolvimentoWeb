<template>
  <v-app>
    <v-app-bar app color="rosa">
      <v-app-bar-nav-icon
        color="white"
        @click="drawer = true"
      ></v-app-bar-nav-icon>
      <v-spacer></v-spacer>
      <div class="d-flex align-center">
        <router-link to="/">
          <v-img
            alt="Logo ICMC"
            class="shrink mt-1"
            contain
            min-width="100"
            src="@/assets/logoICMC.svg"
            width="100"
          />
        </router-link>
      </div>
      <!--<v-spacer></v-spacer>
      <nav>
        <v-btn
          text
          color="white"
          :to="{ name: 'home' }"
          exact-active-class="paginaAtual"
          exact
          ><span class="mr-2">Defesas</span></v-btn
        >

        <v-btn
          text
          color="white"
          :to="{ name: 'quemsomos' }"
          exact-active-class="paginaAtual"
          exact
          ><span class="mr-2">Quem somos</span></v-btn
        >
      </nav>-->
    </v-app-bar>
    <v-navigation-drawer v-model="drawer" absolute temporary>
      <v-list nav dense>
        <v-list-item-group
          v-model="group"
          active-class="rosa--text text--accent-4"
        >
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-table</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Tabela</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-account</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Quem somos</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>
    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    drawer: false,
    group: null,
  }),
};
</script>

<!--<style>
.paginaAtual {
  background: #FF609A;
}
</style>-->