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
      @click="add"
    >
      Submit
    </button>
    <b-table
      :items="dbData"
      :current-page="currentPage"
      :per-page="perPage"
      :fields="fields"
      :sort-by="sortBy"
      :sort-desc="sortDesc"
      class="table-striped">
    </b-table>
    <b-form-group
      label="Per page"
      label-cols-sm="6"
      label-cols-md="4"
      label-cols-lg="3"
      label-align-sm="right"
      label-size="sm"
      label-for="perPageSelect"
      class="mb-0"
    >
      <b-form-select
        v-model="perPage"
        id="perPageSelect"
        size="sm"
        :options="pageOptions"
      ></b-form-select>
    </b-form-group>
    <b-pagination
      v-model="currentPage"
      :total-rows="totalRows"
      :per-page="perPage"
      align="fill"
      size="sm"
      class="my-0"
    ></b-pagination>
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
        fields: [
          { key: 'id', sortable: true , filterByFormatted: true, tdClass: 'd-none', thClass: 'd-none'},
          { key: 'name', sortable: true, filterByFormatted: true },
          { key: 'email', sortable: true },
        ],
        dbData: [],
        totalRows: 1,
        currentPage: 1,
        perPage: 5,
        pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
        sortBy: 'Name',
        sortDesc: false,
        filter: null,
        filterOn: []
      }
    },
    mounted() {
      //
      const db = firebase.firestore()
      db
        .collection('users')
        .get()
        .then(snap => {
          snap.forEach(doc => {
            let tmpdat = {}
            //this.dbData[doc.id] =doc.data()
            this.dbData.push({
              id: doc.id,
              name: doc.data().name,
              email: doc.data().email
            })
            //this.dbData.push({[doc.id]:doc.data()})
          })
        }).then( () => {
        // Set the initial number of items
        this.totalRows = this.dbData.length
      });
      console.log(this.dbData)
    },
    methods: {
      add() {
        const db = firebase.firestore()
        let dbUsers = db.collection('users').add({
            name: this.user.name,
            email: this.user.email,
          })
          .then(ref => {
            this.user.name = ''
            this.user.email = ''
            console.log('added -> Add ID: ', ref.id)
          })
      },
      update(id) {
        const db = firebase.firestore()
        let dbUsers = db.ref('users/' + id)
          .set({
            name: this.user.name,
            email: this.user.email,
          })
          .then(ref => {
            console.log('update -> Add ID: ', ref.id)
          })
      },
    },
  }
</script>
