<template>
    <v-card>
        <v-card-title class="mb-1">
        {{ defesaSelecionada.Nome }}
        </v-card-title>
        <v-card-subtitle class="pb-0">
        {{ defesaSelecionada.Data }}
        </v-card-subtitle>
        <v-card-text>
        <div class="font-weight-bold ms-1">Curso</div>
        <div class="ms-1">{{ defesaSelecionada.Curso }}</div>
        <div class="font-weight-bold ms-1">Programa</div>
        <div class="ms-1">{{ defesaSelecionada.Programa }}</div>
        </v-card-text>
        <v-card-actions>
        <v-btn class="btn-dialog" block @click="$emit('fechar');"
            >Fechar</v-btn
        >
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
  name: "CardFichaIndividual",

  props: {
    defesaSelecionada: Object,
  },
};
</script>