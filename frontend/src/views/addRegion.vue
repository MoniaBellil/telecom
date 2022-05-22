<template>
  <vx-card title="Ajouter Region" code-toggler>
    <div class="mt-5">
      <form @submit.prevent="login" @keydown="form.onKeydown($event)">
        <div class="vx-row">
          <div class="vx-col w-1/2">
            <input v-model="form.name" type="text" name="name" placeholder="Name">
            <div v-if="form.errors.has('name')" v-html="form.errors.get('name')" />
          </div>
          <div class="vx-col w-1/2">
            <select v-model="selected">
              <option value="choisir" >Choisir Central :</option>
              <option v-for="product in central" v-bind:value="{ id: product.id, text: product.name }">{{ product.name }}</option>
            </select>
          </div>
        </div>
        <vs-button type="filled" @click.prevent="addCentrale" class="mt-5 block">Ajouter Central</vs-button>
      </form>
    </div>
  </vx-card>
</template>

<script>
import Form from 'vform'
import axios from "@/axios";

export default {
  data: () => ({
    form: new Form({
      name: '',
      centrale_id:''
    }),
    formUpdate: new Form({
      name: '',
      id:0
    }),
    id:0,
    central:[],
    selected:'choisir',
  }),
  created() {
    if(localStorage.action=="update")
    {
      this.form.name=JSON.parse(localStorage.central).name;
      this.id=JSON.parse(localStorage.central).id;
    }
    axios.get('http://localhost:8000/api/getCentrale')
        .then(res => this.central = res.data)
        .catch(error => error);
  },
  methods: {
    async addCentrale () {
      if (localStorage.action == "update") {
        this.formUpdate.id=this.id;
        this.formUpdate.name=this.form.name;
        axios.post('http://localhost:8000/api/updateCentrale', this.formUpdate, {
          headers: {'content-type': 'application/json'}
          , useCredentails: true
        })
            .then(response => {
              if (response.status === 200) {
                this.$router.push('/Centrale');
              }
            })
            .catch(error => {
              // code here when an upload is not valid
              this.uploading = false
              this.error = error.response.data
              console.log('check error: ', this.error)
            });
      } else {
        this.form.centrale_id=this.selected.id;
        console.log(this.form);
        axios.post('http://localhost:8000/api/addRegion', this.form, {
          headers: {'content-type': 'application/json'}
          , useCredentails: true
        })
            .then(response => {
              if (response.status === 200) {
                this.$router.push('/Region');
              }
            })
            .catch(error => {
              // code here when an upload is not valid
              this.uploading = false
              this.error = error.response.data
              console.log('check error: ', this.error)
            });
      }
    }
  }
}
</script>
