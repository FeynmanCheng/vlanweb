<template>
  <v-app>
    <v-app-bar app color="primary" dark class="justify-space-around" dense>
      <span>VLAN 单臂路由</span>
    </v-app-bar>

    <v-main>
      <Console />
    </v-main>
  </v-app>
</template>

<script>
import Console from './components/Console'

export default {
  name: 'App',

  components: {
    Console,
  },s

  data: () => ({
    //
  }),
}
</script>
