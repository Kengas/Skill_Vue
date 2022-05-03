<template>
  <div class="container">
    <div class="large-12 medium-12 small-12 cell">
<!-- 
    <v-file-input
        v-model="file"
        id="file"
        ref="file"
        label="File input"
        outlined
        dense
        multiple
         @change="handleFileUpload($event)"
    ></v-file-input> -->

      <label>File
        <input id="file" ref="file" type="file" @change="handleFileUpload($event)">
      </label>
      <v-card height="40%" class="grey lighten-4 paddign">
        <img :src="file" width="10%">
        <v-card-actions style="font-size:100%">
          <span><i class="fas fa-image fa-2x" /></span>
          <v-spacer />
          <span>รูปภาพ</span>
        </v-card-actions>
      </v-card>
      <button @click="submitFile()">
        Submit
      </button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      file: ''
    }
  },
  methods: {
    async submitFile () {
      const formData = new FormData()      
      formData.append('file', this.$refs.file.files[0])      
      
      console.log(`file=> `, this.$refs.file.files[0])
      // console.log(`formdata =>`  ,  formData)
      const res = await axios.post('http://localhost:4500/upload', formData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      })
      console.log('ok=>', res.data)
    },
    handleFileUpload (e) {
      // this.file = this.$refs.file.files[0]
      const image = e.target.files[0]
      console.log('e.target.files[0] =>' ,e.target.files[0] )
      const reader = new FileReader()
      reader.readAsDataURL(image)
      reader.onload = (e) => {
        this.file = e.target.result
        // console.log('file', this.file)
      }
    //   console.log('file', this.file)
    }
  }
}
</script>