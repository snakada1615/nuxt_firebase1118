<template>
  <div class="page">
    <label>
     <span>
       お名前:
     </span>
      <input
        type="text"
        v-model="user.name"
      >
    </label>
    <label>
     <span>
       email:
     </span>
      <input
        type="text"
        v-model="user.email"
      >
    </label>
    <button
      type="button"
      @click="submit"
    >
      Submit
    </button>
    <button
      type="button"
      @click="getData"
    >
      getchData
    </button>
    <p>{{ dbData }}</p>
  </div>
</template>

<script>
  import firebase from '@/plugins/firebase'

  export default {
    data () {
      return {
        user: {
          name: "",
          email: ""
        },
        dbData: "",
      }
    },
    methods: {
      submit () {
        const db = firebase.firestore()
        let dbUsers = db.collection('users')
        dbUsers
          .add({
            name: this.user.name,
            email: this.user.email,
          })
          .then(ref => {
            console.log('Add ID: ', ref.id)
          })
      },
      getData () {
        const db = firebase.firestore()
        let docUsers = db.collection('users').doc('8Ner2tNFSFmXYq3hDYck')
        let dbData = []
        this.dbData = dbData
        docUsers
          .get()
          .then(function(doc) {
            dbData.push(doc.data().name)
          })
      },
    },
  }
</script>
