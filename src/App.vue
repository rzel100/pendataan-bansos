<template>
  <v-app>
    <div>
    <v-app-bar
      color="green"
      elevation="4"
      dark
      dense>
    <v-icon class="pa-4">mdi-pencil</v-icon>
    <v-toolbar-title>Input Pendataan Bansos</v-toolbar-title>
    </v-app-bar>
    </div>

    <v-main>
      <Form/>
    </v-main>
  </v-app>
</template>

<script>
import Form from './components/Form';

export default {
  components: {
    Form,
  }
};
</script>
