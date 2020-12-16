<template>
  <v-container>
    <v-row class="text-center">

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Console
        </h1>
        <v-text-field v-model="command" outlined clearable label="$>">
        </v-text-field>
        <v-btn color="primary" @click="sendCommand">Confirm</v-btn>
      </v-col>

      <v-col
        class="mb-5"
        cols="12"
      >
        <v-textarea clearable readonly :value="output" outlined>

        </v-textarea>
      </v-col>

    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',

    data(){
      return{
        command: '',
        output: 'output'
      }
    },
    methods:{
      sendCommand(){
        console.log(this.command)
      }
    },
    beforeCreate() {

    }
  }
</script>
