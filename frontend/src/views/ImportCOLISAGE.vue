<template>
  <div>
    <section>
      <vs-button class="float-right" v-on:click="save">Save Data</vs-button>
      <center>
        <h3>Import Colisage</h3>
        <br/>
        <input type="file" @change="onChange" />
        <br/>
        <xlsx-read :file="file">
          <xlsx-sheets>
            <template #default="{sheets}">
              <br/>
              <select v-model="selectedSheet">
                <option v-for="sheet in sheets" :key="sheet" :value="sheet">
                  {{ sheet }}
                </option>
              </select>
            </template>
          </xlsx-sheets>
          <xlsx-table :sheet="selectedSheet" />
        </xlsx-read>
      </center>
    </section>
  </div>
</template>

<script>

import {XlsxDownload, XlsxJson, XlsxRead, XlsxSheet, XlsxSheets, XlsxTable, XlsxWorkbook} from "vue-xlsx";
import axios from "@/axios";

export default {
  components: {
    XlsxRead,
    XlsxTable,
    XlsxSheets,
    XlsxJson,
    XlsxWorkbook,
    XlsxSheet,
    XlsxDownload
  },
  data() {
    return {
      file: null,
      selectedSheet: null,
      sheetName: null,
      sheets: [{ name: "SheetOne", data: [{ c: 2 }] }],
      collection: [{ a: 1, b: 2 }]
    };
  },
  methods: {
    onChange(event) {
      this.file = event.target.files ? event.target.files[0] : null;
    },
    addSheet() {
      this.sheets.push({ name: this.sheetName, data: [...this.collection] });
    },
    save() {
      let formData = new FormData();
      formData.append('import_file', this.file);
      formData.append('sheet', this.selectedSheet.toString());
      axios.post('http://localhost:8000/api/users/importPacking', formData, {
        headers: { 'content-type': 'multipart/form-data' }
        ,  useCredentails: true })
          .then(response => {
            if(response.status === 200) {
              // codes here after the file is upload successfully
            }
          })
          .catch(error => {
            // code here when an upload is not valid
            this.uploading = false
            this.error = error.response.data
            console.log('check error: ', this.error)
          });
    },
  }
};
</script>
