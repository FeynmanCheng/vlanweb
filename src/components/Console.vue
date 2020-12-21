<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <span class="display-1 font-weight-bold mb-3">
          Console {{ currentPrompt }}
          <v-chip :color="colorStatus" label class="mb-2">{{ status }}</v-chip>
        </span>

        <v-text-field v-model="command" outlined clearable autofocus label="$>" @keydown.enter="sendCommand">
        </v-text-field>
      </v-col>

      <v-col
          class="mb-5"
          cols="12"
      >
        <v-textarea id="text" clearable readonly :value="output" outlined height="50vh">

        </v-textarea>
      </v-col>
      <v-row justify="space-between">
        <v-btn v-for="t in terminals" :key="t" color="primary" @click="changePrompt(t)">{{ t }}</v-btn>
      </v-row>

    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',

  data() {
    return {
      command: '',
      output: '',
      terminals: ['S1', 'S2', 'S3', 'R', 'T'],
      currentPrompt: 'T',
      status: 'Connecting',
      colorStatus: 'warning',
    }
  },

  methods: {
    login(){
      this.axios.get("/login/" + this.currentPrompt).then(
        res => {
          this.output = res.data
          this.$nextTick(() => {
            let t = document.getElementById("text")
            t.scrollTop = t.scrollHeight
          })
          this.changeStatus("Connected")
        }
      ).catch(() =>{
        this.changeStatus("Error")
      })
    },

    sendCommand() {
      this.changeStatus("Pocessing")
      this.axios.post("/command/" + this.currentPrompt, {
        cmd: this.command
      }).then(res => {
        this.output += res.data
        this.$nextTick(() => {
          let t = document.getElementById("text")
          t.scrollTop = t.scrollHeight
          this.changeStatus("ok")
        })
      }).catch(() =>{
        this.changeStatus("Error")
      })
      this.command = ''


    },
    changePrompt(prompt) {
      this.changeStatus('Connecting')
      this.currentPrompt = prompt
      this.output = ''
      this.login()
    },
    changeStatus(status) {
      this.status = status

      if (this.status === "Connected" || this.status === "ok") {
        this.colorStatus = "success"
      } else if (this.status === "Connecting" || this.status === "Pocessing"){
        this.colorStatus = "warning"
      }else if (this.status === "Error"){
        this.colorStatus = "error"
      }
    }

  },
  beforeMount() {
    this.login()
  }
}
</script>
