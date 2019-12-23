<template>
    <v-container>
        <v-card>
            <v-toolbar dense color="success" dark flat>
                <v-toolbar-title>list</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn icon @click="list">
                    <v-icon>mdi-refresh</v-icon>
                </v-btn>
            </v-toolbar>
            <v-card-text>
              <v-list-item v-for="item in items" three-line :key="item.id">
                <v-list-item-content>
                  <v-list-item-title>{{item.title}}</v-list-item-title>
                  <v-list-item-subtitle>{{item.description}}</v-list-item-subtitle>
                  <v-list-item-subtitle>{{item.date}}</v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn :to="`/doc/${item.category}/${item.name}`" icon>
                    <v-icon>mdi-arrow-right</v-icon>
                  </v-btn>
                </v-list-item-action>
              </v-list-item>
            </v-card-text>
            <v-radio-group v-model="radioGroup">
              <v-radio
                v-for="n in 3"
                :key="n"
                :label="`Radio ${n}`"
                :value="n"
              ></v-radio>
            </v-radio-group>
        </v-card>
    </v-container>
</template>

<script>
export default {
  data () {
    return {
      items: [],
      radioGroup: 1
    }
  },
  mounted () {
    this.list()
  },
  methods: {
    async list () {
      const sn = await this.$fireStore.collection('docs').get()
      sn.docs.forEach((v) => {
        const item = v.data()
        item.id = v.id
        item.category = v.id.split('-')[0]
        item.name = v.id.split('-')[1]
        this.items.push(item)
      })
    }
  }
}
</script>
