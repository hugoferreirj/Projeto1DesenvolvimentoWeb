<template>
  <v-app>
    <v-app-bar app color="rosa" class="elevation-2">
      <!--<v-app-bar-nav-icon
        color="white"
        @click="drawer = true"
      ></v-app-bar-nav-icon>
      <v-spacer></v-spacer>-->
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
      <template>
        <v-tabs color="white" class="ms-4">
          <v-tabs-slider color="white"></v-tabs-slider>
          <v-tab :to="{ name: 'home' }" exact> Tabela </v-tab>
          <v-tab :to="{ name: 'quemsomos' }" exact> Quem somos </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>
    <!--<v-navigation-drawer v-model="drawer" app temporary>
      <v-list nav dense>
        <v-list-item-group
          v-model="group"
          active-class="rosa--text text--accent-4"
        >
          <v-list-item :to="{ name: 'home' }" exact>
            <v-list-item-icon>
              <v-icon>mdi-table</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Tabela</v-list-item-title>
          </v-list-item>
          <v-list-item :to="{ name: 'quemsomos' }" exact>
            <v-list-item-icon>
              <v-icon>mdi-account-group</v-icon>
            </v-list-item-icon>
            <v-list-item-title>Quem somos</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>-->
    <v-main class="corDeFundo">
      <router-view />
    </v-main>
    <v-footer
      color="laranja"
      class="text-center d-flex flex-column"
      height="100"
    >
      <div><strong>SSC0961 - Desenvolvimento Web e Mobile</strong></div>
      <v-divider></v-divider>
      <div>Docente: Adenilso da Silva Simão</div>
      <div>Estagiário PAE: Rodrigo Henrique Ramos</div>
    </v-footer>
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

<style>
.corDeFundo {
  background: #ffd152;
}
</style>