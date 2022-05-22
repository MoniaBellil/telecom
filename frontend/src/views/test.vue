<template>
  <div>
    <select class="col" v-model="selectedCentrale" @change="onChange($event)">
      <option value="choisirCentral">Choisir Centrale :</option>
      <option  v-for="of in centrale" :value="of">{{of.name}}</option>
    </select>
    <br/>
    <select class="col" v-model="selectedRegion" @change="onChange1($event)">
      <option value="choisirRegion">Choisir Region :</option>
      <option  v-for="of in region" :value="of">{{of.name}}</option>
    </select>
    <br/>
    <select v-model="selectedLiaison" class="col" @change="onChange2($event)">
      <option value="choisirLiaison">Choisir Liaison :</option>
      <option  v-for="of in liaison" :value="of">{{of.name}}</option>
    </select>
    <l-map
        :zoom="zoom"
        :center="center"
        style="height: 80vh"
    >
      <l-tile-layer
          :url="url">
      </l-tile-layer>
      <l-marker v-for="elemnt in markes" :lat-lng="elemnt" ></l-marker>
      <l-polyline
          :lat-lngs="markes"
          color="green"></l-polyline>
    </l-map>
  </div>
</template>
<script>
import Vue from 'vue';
import { MapsPlugin, NavigationLine, Marker, Zoom } from '@syncfusion/ej2-vue-maps';
import axios from "@/axios";
Vue.use(MapsPlugin);
import {LMap, LTileLayer, LMarker} from 'vue2-leaflet';
import LPolyline from "vue2-leaflet/dist/components/LPolyline";

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolyline,
  },
  data () {
    return{
      markes:[],
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      zoom: 7,
      center: [34.811,9.558],
      selectedCentrale:'choisirCentral',
      selectedRegion:'choisirRegion',
      selectedLiaison:'choisirLiaison',
      liaison:[],
      form:[
        {id: 0}
      ],
      layerType: 'OSM',
      width: 5,
      angle: 0.1,
      latitude: [35.7073, 36.6463],
      longitude: [10.1508,10.151],
      centrale:[],
      region:[]
    }
  },
  created() {
    axios.get('http://localhost:8000/api/getCentrale')
        .then(res => this.centrale = res.data)
        .catch(error => error);
  },
  provide: {
    maps: [ NavigationLine, Marker, Zoom ]
  },
  methods: {
    async onChange(event) {
      axios.post('http://localhost:8000/api/getRegionById', {'id':this.selectedCentrale.id}, {
        headers: {'content-type': 'application/json'}
        , useCredentails: true
      })
          .then(response => {
            if (response.status === 200) {
              this.region = response.data;
              this.center=L.latLng(36.801747,10.1635008);
              this.center=[36.801747,10.1635008];
            }
          });
      this.zoom=13;
    },onChange1(event) {
      axios.post('http://localhost:8000/api/getLiaisonByRegion', {'id':this.selectedRegion.id}, {
        headers: {'content-type': 'application/json'}
        , useCredentails: true
      })
          .then(response => {
            if (response.status === 200) {
              this.liaison = response.data;
              this.center=L.latLng(36.8093006,10.1345729);
              this.center=[36.8093006,10.1345729];
            }
          });
    },onChange2(event) {
      axios.post('http://localhost:8000/api/getChambreByLiaison', {'id':this.selectedLiaison.id}, {
        headers: {'content-type': 'application/json'}
        , useCredentails: true
      })
          .then(response => {
            if (response.status === 200) {
              for(var ele in response.data)
              {
                this.markes.push(L.latLng(response.data[ele].coord_x,response.data[ele].coord_y));
              }
            }
          });
    }
  }
}
</script>
<style>
.wrapper {
  max-width: 400px;
  margin: 0 auto;
}
</style>
