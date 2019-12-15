<template>
    <v-card>{{ doc }}</v-card>
</template>

<script>
export default {
  data () {
    return {
      doc: ''
    }
  },
  async mounted () {
    console.log(this.$route.params.category)
    const id = this.$route.params.category + '-' + this.$route.params.name
    const sn = await this.$fireStore.collection('docs').doc(id).get()
    this.doc = sn.data()
  }
}
</script>
