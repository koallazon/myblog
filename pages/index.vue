<template>
  <v-container fluid>
    <v-card>
      <v-card-text>
        <v-textarea v-model="str" />
      </v-card-text>
    </v-card>
    <v-card>
      <v-card-text v-html="$md.render(str)" style="white-space:pre" />
    </v-card>
  </v-container>
</template>

<script>
import readMe from '../README.md'
export default {
  data () {
    return {
      items: [],
      str: ''
    }
  },
  coumputed: {
    hello () {
      return readMe
    }
  }
}
</script>
