<template>
  <vx-card title="Ajouter Central" code-toggler>
    <div class="mt-5">
      <form @submit.prevent="login" @keydown="form.onKeydown($event)">
        <div class="vx-row">
          <div class="vx-col w-1/2">
            <input v-model="form.name" type="text" name="name" placeholder="Name">
            <div v-if="form.errors.has('name')" v-html="form.errors.get('name')" />
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
    }),
    formUpdate: new Form({
      name: '',
      id:0
    }),
    id:0
  }),
  created() {
    if(localStorage.action=="update")
    {
      this.form.name=JSON.parse(localStorage.central).name;
      this.id=JSON.parse(localStorage.central).id;
    }
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
        axios.post('http://localhost:8000/api/addCentrale', this.form, {
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
      }
    }
  }
}
</script>
