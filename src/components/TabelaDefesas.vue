<template>
  <div>
    <v-container>
      <!--<template>
        <p>{{ defesas }}</p>
      </template>-->
      <v-card>
        <v-card-title>
          <v-text-field
            v-model="buscar"
            color="laranja"
            append-icon="mdi-magnify"
            label="Buscar por nome"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table
          v-model="defesaSelecionada"
          :headers="cabecalhoTabela"
          :items="defesas"
          :items-per-page="10"
          class="elevation-1"
          :search="buscar"
          :loading="carregando"
          single-select
          show-select
          @input="visualizarFichaIndividual()"
          item-key="Nome"
          loading-text="Carregando dados..."
          checkbox-color="laranja"
        >
          ><template slot="progress">
            <v-progress-linear
              color="laranja"
              indeterminate
            ></v-progress-linear> </template
        ></v-data-table>
      </v-card>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "TabelaDefesas",
  data() {
    return {
      buscar: "",
      defesaSelecionada: undefined,
      defesas: [],
      carregando: true,
      cabecalhoTabela: [
        { text: "Nome", value: "Nome", filterable: true },
        { text: "Curso", value: "Curso", filterable: false },
        { text: "Programa", value: "Programa", filterable: false },
        { text: "Data", value: "Data", filterable: false },
        /*{ text: "Orientador", value: "Orientador", filterable: false },*/
      ],
    };
  },
  methods: {
    buscarDefesas() {
      const url = "http://thanos.icmc.usp.br:4567/api/v1/defesas";
      fetch(url)
        .then((data) => data.json())
        .then((response) => {
          this.defesas = response.items;
          this.carregando = false;
        });
    },
    visualizarFichaIndividual() {
      console.log(this.defesaSelecionada[0]);
    },
  },
  beforeMount() {
    this.buscarDefesas();
  },
};
</script>


<!--<template>
  <div>
    <v-btn :loading="loading" class="primary" @click="loadPersonagens()"
      >Load!</v-btn
    >
    <template v-if="personagem">
      <v-card>
        <v-card-title> {{ personagem.fullName }} </v-card-title>
        <v-card-text>
          <v-img :src="personagem.imageUrl"> </v-img>
        </v-card-text>
        <v-card-actions>
          <v-icon @click="personagem = undefined">mdi-delete</v-icon>
        </v-card-actions>
      </v-card>
    </template>
    <template v-if="personagensSelecionado">
      <v-container>
        <v-row>
          <v-col cols="12">
            <v-text-field
              label="Digite parte do nome para filtrar"
              :rules="regrasNome"
              v-model="filter"
            >
            </v-text-field>
          </v-col>
        </v-row>
        <template v-for="p in personagensSelecionado">
          <v-row :key="p.id">
            <v-col cols="8"> {{ p.fullName }} </v-col>
            <v-col cols="4">
              <v-icon @click="personagem = p">mdi-download</v-icon>
            </v-col>
          </v-row>
        </template>
      </v-container>
    </template>
  </div>
</template>

<script>
export default {
  name: "TabelaDefesas",
  data() {
    return {
      personagem: undefined,
      filter: "",
      personagens: [],
      regrasNome: [
        (value) => value.length >= 3 || "Digite ao menos 3 letras",
        (value) => value !== "Drogo" || "Drogo nao por favor!!",
      ],
      loading: false,
    };
  },
  methods: {
    loadPersonagens() {
      this.loading = true;
      const url = "https://thronesapi.com/api/v2/Characters";
      fetch(url)
        .then((data) => data.json())
        .then((response) => {
          this.personagens = response;
          this.loading = false;
        });
    },
  },
  computed: {
    personagensSelecionado() {
      return this.personagens.filter((e) => e.fullName.match(this.filter));
    },
  },
};
</script>-->

