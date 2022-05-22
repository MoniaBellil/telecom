<!-- =========================================================================================
    File Name: TableEditDataTable.vue
    Description: You can also edit data with edit slot
    ----------------------------------------------------------------------------------------
    Item Name: Vuexy - Vuejs, HTML & Laravel Admin Dashboard Template
      Author: Pixinvent
    Author URL: http://www.themeforest.net/user/pixinvent
========================================================================================== -->


<template>
  <vx-card title="Liste Chambre" code-toggler>
    <vs-button type="filled" @click.prevent="goToAdd" class="mt-5 block">Ajouter Chambre</vs-button>
    <vs-table :data="users">
      <template slot="thead">
        <vs-th>Type</vs-th>
        <vs-th>Coordonnée x</vs-th>
        <vs-th>Coordonnée Y</vs-th>
        <vs-th>Etat</vs-th>
        <vs-th>Liaison</vs-th>
        <vs-th>updated_at</vs-th>
        <vs-th>created_at</vs-th>
        <vs-th>Action</vs-th>
      </template>

      <template slot-scope="{data}">
        <vs-tr :key="indextr" v-for="(tr, indextr) in data">

          <vs-td :data="tr.type">
            {{tr.type}}
          </vs-td>

          <vs-td :data="tr.coord_x">
            {{tr.coord_x}}
          </vs-td>

          <vs-td :data="tr.coord_y">
            {{tr.coord_y}}
          </vs-td>

          <vs-td :data="tr.etat">
            {{tr.etat}}
          </vs-td>

          <vs-td :data="tr.liaison_id">
            bardo1_omransuperieur
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

export default {
  data () {
    return {
      users: [],
      form: [
        {id:0}
      ]
    }
  },
  created() {
    axios.get('http://127.0.0.1:8000/api/getChambre')
        .then(res => this.users = res.data)
        .catch(error => error);
  },
  methods: {
    getName(id) {
      this.form.id=id;
      axios.post('http://localhost:8000/api/getLiaisonChambre', {"id":1}, {
        headers: {'content-type': 'application/json'}, useCredentails: true})
          .then(response => {
            if (response.status === 200) {
              this.name = response.data
            }
          });
      return this.name;
    },
  }
}
</script>
