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
    <ul>
      <li :key="item.key" v-for="item in dbData">{{item}}</li>
    </ul>
  </div>
</template>

<script>
  import firebase from '@/plugins/firebase'

  export default {
    data() {
      return {
        user: {
          name: "",
          email: ""
        },
        dbData: [],
      }
    },
    mounted() {
      const db = firebase.firestore()
      db
        .collection('users')
        .get()
        .then(snap => {
          const dbData = [];
          snap.forEach(doc => {
            dbData.push({[doc.id]: doc.data()});
          });
          this.dbData = dbData;
        });
    },
    methods: {
      submit() {
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
      getData() {
        console.log('ok01')
        var vue = this;
        firebase.database().ref('users').on('value', function (snapshot) {
          vue.dbData = snapshot.val();
        });
      }
    },
  }
</script>
