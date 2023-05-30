<template>
  <v-app>
    <v-app-bar app color="rosa">
      <div class="d-flex align-center">
        <v-img
          alt="Logo ICMC"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="@/assets/logoICMC.svg"
          width="100"
        />
      </div>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
