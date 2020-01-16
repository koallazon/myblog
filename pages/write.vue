<template>
    <v-container fluid>
        <v-card v-if="user">
            <v-toolbar dense color="success" dark flat>
                <v-toolbar-title>write</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-btn icon @click="save">
                    <v-icon>mdi-content-save</v-icon>
                </v-btn>
                <v-btn icon @click="signOut">
                    <v-icon>mdi-account-box</v-icon>
                </v-btn>
            </v-toolbar>
            <v-form>
                <v-card-text>
                    <v-row>
                        <v-col cols="12" sm="6">
                          <v-text-field v-model="docId.category" label="category" outlined />
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-text-field v-model="docId.name" label="name" outlined />
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-text-field v-model="doc.title" label="title" outlined />
                        </v-col>
                        <v-col cols="12" sm="6">
                          <v-text-field v-model="doc.date" label="date" outlined />
                        </v-col>
                        <v-col cols="12">
                          <v-text-field v-model="doc.description" label="description" outlined />
                        </v-col>
                        <v-col cols="12">
                          <v-textarea v-model="content.text" label="content" rows="20" outlined />
                        </v-col>
                    </v-row>
                </v-card-text>
            </v-form>
        </v-card>
        <v-card v-else>
          <v-card-text class="text-center">
            <v-btn @click="signIn">
              signIn
            </v-btn>
          </v-card-text>
        </v-card>
    </v-container>
</template>
<script>
export default {
  data () {
    return {
      docId: {
        category: '',
        name: ''
      },
      doc: {
        createAt: new Date(),
        title: '',
        date: '',
        description: '',
        tags: []
      },
      content: {
        createdAt: new Date(),
        modifiedAt: new Date(),
        text: ''
      },
      user: null
    }
  },
  methods: {
    async save () {
      const id = this.docId.category + '-' + this.docId.name
      this.doc.createdAt = new Date()
      await this.$fireStore.collection('docs').doc(id).set(this.doc)
      const cid = id + '/content/last'
      this.content.createdAt = new Date()
      this.content.modifiedAt = new Date()
      await this.$fireStore.collection('docs').doc(cid).set(this.content)
      alert('저장완료')
    },
    async signIn () {
      try {
        this.loading = true
        const provider = new this.$fireAuthObj.GoogleAuthProvider()
        const auth = await this.$fireAuth.signInWithPopup(provider)
        this.user = auth.user
      } catch (e) {
        console.error(e.message)
      } finally {
        this.loading = false
      }
    },
    async signOut () {
      try {
        this.loading = true
        await this.$fireAuth.signOut()
        this.user = null
      } catch (e) {
        console.error(e.message)
      } finally {
        this.loading = false
      }
    }
  }
}
</script>
