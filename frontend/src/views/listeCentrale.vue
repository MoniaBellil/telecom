<!-- =========================================================================================
    File Name: TableEditDataTable.vue
    Description: You can also edit data with edit slot
    ----------------------------------------------------------------------------------------
    Item Name: Vuexy - Vuejs, HTML & Laravel Admin Dashboard Template
      Author: Pixinvent
    Author URL: http://www.themeforest.net/user/pixinvent
========================================================================================== -->


<template>
  <vx-card title="Liste Centrale" code-toggler>
    <vs-button type="filled" @click.prevent="goToAdd" class="mt-5 block">Ajouter Central</vs-button>
    <vs-table :data="users">
      <template slot="thead">
        <vs-th>Id</vs-th>
        <vs-th>Nom</vs-th>
        <vs-th>updated_at</vs-th>
        <vs-th>created_at</vs-th>
        <vs-th>Action</vs-th>
      </template>

      <template slot-scope="{data}">
        <vs-tr :key="indextr" v-for="(tr, indextr) in data">

          <vs-td :data="tr.id">
            {{tr.id}}
          </vs-td>

          <vs-td :data="tr.name">
            {{tr.name}}
          </vs-td>

          <vs-td :data="tr.created_at">
            {{tr.created_at}}
          </vs-td>

          <vs-td :data="tr.created_at">
            {{tr.created_at}}
          </vs-td>

          <vs-td class="whitespace-no-wrap">
            <feather-icon icon="EditIcon" svgClasses="w-5 h-5 hover:text-primary stroke-current" @click.stop="editData(tr)" />
            <feather-icon icon="TrashIcon" svgClasses="w-5 h-5 hover:text-danger stroke-current" class="ml-2" @click.stop="deleteData(tr.id)" />
          </vs-td>

        </vs-tr>
      </template>
    </vs-table>

  </vx-card>
</template>

<script>

import axios from "@/axios";
import Form from "vform";

export default {
  data () {
    return {
      users: [],
      form:new Form({
        id: '',
      })
    }
  },
  created() {
    axios.get('http://localhost:8000/api/getCentrale')
        .then(res => this.users = res.data)
        .catch(error => error);
  },
  methods:{
    goToAdd(){
      localStorage.action="ajoute";
      this.$router.push('/ajouterCentral');
    },
    deleteData (id) {
      this.form.id=id;
      axios.post('http://localhost:8000/api/DeleteCentrale', this.form, {
        headers: { 'content-type': 'application/json' }
        , useCredentails: true })
          .then(response => {
            if(response.status === 200) {
              this.goBack();
            }
          })
          .catch(error => {
            // code here when an upload is not valid
            this.uploading = false
            this.error = error.response.data
            console.log('check error: ', this.error)
          });
      //this.$store.dispatch('dataList/removeItem', id).catch(err => { console.error(err) })
    },
    editData(central)
    {
      var personJSONString=JSON.stringify(central);
      localStorage.central=personJSONString;
      localStorage.action="update";
      this.$router.push('/ajouterCentral');
    }

  }
}
</script>
