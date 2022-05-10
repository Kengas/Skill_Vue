<template>
<div class="text-center">

        <label>File1
          <input style="display:none;" id="file1" ref="file1" type="file" accept="image/*" @change="handleFileUpload1($event),submitFile1()" multiple>
        </label>
        <v-card height="10%" class="grey lighten-4 paddign">
          <!-- <img :src='file1' > ทำไมต้องเป็น file1 เป็นตัวอื่นได้มั้ย -->
          <img :src="file1" width="20%">
          <v-card-actions style="font-size:100%">
            <span><i class="fas fa-image fa-2x" /></span>
            <v-spacer />
            <span>รูปภาพ</span>
          </v-card-actions>
        </v-card>

        <label>File2
          <input style="display:none;" id="file2" ref="file2" type="file" accept="image/*" @change="handleFileUpload2($event),submitFile2()" multiple>
        </label>
        <v-card height="10%" class="grey lighten-4 paddign">
          <img :src="file2" width="20%">
          <v-card-actions style="font-size:100%">
            <span><i class="fas fa-image fa-2x" /></span>
            <v-spacer />
            <span>รูปภาพ</span>
          </v-card-actions>
        </v-card>

        <label>File3
          <input style="display:none;" id="file3" ref="file3" type="file" accept="image/*" @change="handleFileUpload3($event),submitFile3()" multiple>
        </label>
        <v-card height="10%" class="grey lighten-4 paddign">
          <img :src="file3" width="20%">
          <v-card-actions style="font-size:100%">
            <span><i class="fas fa-image fa-2x" /></span>
            <v-spacer />
            <span>รูปภาพ</span>
          </v-card-actions>
        </v-card>

    <v-dialog
      v-model="dialog"
      width="500"
    >    

      <v-card>
        <v-card-title class="text-h5 red">
          แจ้งเตือน
        </v-card-title>

        <v-card-text>
          {{ msg }}
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            I accept
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

  <v-row justify="center">
    <v-col
      cols="12"
      sm="10"
      md="8"
      lg="6"
    >
      <v-card ref="form">        

        <v-card-text>

          <v-text-field
            ref="std_id"
            v-model="std_id"
            label="รหัสนักศึกษา"            
            required
          ></v-text-field>

          <v-text-field
            ref="title"
            v-model="title"
            label="คำนำหน้า"            
            required
          ></v-text-field>

          <v-text-field
            ref="fname"
            v-model="fname"
            label="ชื่อ"            
            required
          ></v-text-field>

          <v-text-field
            ref="lname"
            v-model="lname"
            label="นามสกุล"            
            required
          ></v-text-field>
          
          <v-text-field
            ref="pass"
            v-model="pass"
            label="รหัสผ่าน"            
            required
          ></v-text-field>
          
          <v-text-field
            ref="teacher_id"
            v-model="teacher_id"
            label="รหัสครูที่ปรึกษา"            
            required
          ></v-text-field>
          
          <v-text-field
            ref="dep_id"
            v-model="dep_id"
            label="รหัสครูที่แผนก"            
            required
          ></v-text-field>
          
          <v-text-field
            ref="img"
            v-model="img"
            label="รูปประจำตัว"            
            required
          >
          </v-text-field>        
          
        </v-card-text>
        <v-divider class="mt-12"></v-divider>
        <v-card-actions>
          <v-btn text  @click="cancel">
            Cancel
          </v-btn>
          <v-spacer></v-spacer>          
          <v-btn
            color="primary"
            text
            @click="submit"
          >
            Submit
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</div>
</template>

<script>
import axios from 'axios'
  export default {
    data: () => ({
      std_id: '',
      title: '',
      fname: '',
      lname: '',
      dep_id: '',
      teacher_id: '',
      pass: '',
      img: 'avartar.jpg',
      dialog:false,
      msg: '',
      status: '',
      np: '',
      file1: '',
      file2: '',
      file3: '',
      test1: '',
      test2: '',
      test3: '',

    }),

    watch: {
      std_id (){
        this.errorMessages = ''
      },
    },

    methods: {
      async submit(){
        try{
          const std = {
          std_id: this.std_id,
          fname: this.fname,
          lname: this.lname,
          dep_id: this.dep_id,
          teacher_id: this.teacher_id,
          pass: this.pass,
          img1: this.test1,
          img2: this.test2,
          img3: this.test3,
          title: this.title,
        }
          console.log('user:' , std)
          const res = await axios.post('http://localhost:4500/save',std)
          console.log('data=', res.data.msg)
          this.msg = res.data.msg
          if(res.data.status == 0){

            console.log('ID has exists')
            this.dialog = true
            setTimeout(()=>{
              this.dialog = false
              // this.$router.push('/list')
              console.log('0')
              return
            },2000)

          }else{

            console.log('save data')
            this.dialog = true
            setTimeout(()=>{
              this.dialog = false
              console.log('1')
              // this.$router.push('/list')
              return
            },2000)
            
          }
        }catch(e){
          console.log(e.message)
        }
      },
      async cancel(){
        console.log('cancel')
         this.$router.push('/list')
      },
      // อัพโหลดข้อมูล
      async submitFile1(){
       try{
          const formData = new FormData() // new ใช้สำหรับสร้าง object หรือ arryy ( ไม่แน่ใจว่า array ด้วยหรือป่าว )
          formData.append(`file` , this.$refs.file1.files[0] )
          const res = await axios.post('http://localhost:4500/upload' , formData , {
            headers: {
              'content-type' : 'multipart/form-data'
            }
          })
          this.test1 = res.data.filesname
       }catch(e){
          console.log(e.message)
       }
      },
      // อัพโหลดข้อมูล
      async submitFile2 () {
       try{
          const formData = new FormData() // new ใช้สำหรับสร้าง object หรือ arryy ( ไม่แน่ใจว่า array ด้วยหรือป่าว )
          formData.append(`file` , this.$refs.file2.files[0] )
          console.log(`$refs => ` , this.$refs.file2.files )
          console.log(`formData => ` , this.$refs.file2.files[0]  )

          const res = await axios.post('http://localhost:4500/upload' , formData , {
            headers: {
              'content-type' : 'multipart/form-data'
            }
          })
          console.log(`ok1 => ` , res.data)
          console.log(`ok2 => ` , res.data.filesname)          
          this.test2 = res.data.filesname
          // this.file1.files[0].name = this.test2
          // console.log(`test => ` , this.test2)
       }catch(e){
          console.log(e.message)
       }
      },
      // อัพโหลดข้อมูล
      async submitFile3 () {
       try{
          const formData = new FormData() // new ใช้สำหรับสร้าง object หรือ arryy ( ไม่แน่ใจว่า array ด้วยหรือป่าว )
          formData.append(`file` , this.$refs.file3.files[0] )
          console.log(`$refs => ` , this.$refs.file3.files )
          console.log(`formData => ` , this.$refs.file3.files[0]  )

          const res = await axios.post('http://localhost:4500/upload' , formData , {
            headers: {
              'content-type' : 'multipart/form-data'
            }
          })
          console.log(`ok1 => ` , res.data)
          console.log(`ok2 => ` , res.data.filesname)          
          this.test3 = res.data.filesname
       }catch(e){
          console.log(e.message)
       }
      },
    handleFileUpload1(e) {
      const image = e.target.files[0]
      console.log('image => ',image)
      const reader = new FileReader()
      console.log('reader => ' , reader)
      reader.readAsDataURL(image)
      reader.onload = (e) => {
        this.file1 = e.target.result
      }
    },
    handleFileUpload2 (e) {
      // this.file = this.$refs.file.files[0]
      const image = e.target.files[0]
      console.log('image2 =>' ,e.target.files)
      console.log('file2 =>' , this.$refs.file2.files[0].name)
      this.pic2 = this.$refs.file2.files[0].name
      const reader = new FileReader()
      reader.readAsDataURL(image)
      reader.onload = (e) => {
        this.file2= e.target.result
        // console.log('file', e.target.result)      
      }
    //   console.log('file', this.file)
    },
    handleFileUpload3 (e) {
      // this.file = this.$refs.file.files[0]
      const image = e.target.files[0]
      console.log('image3 =>' ,e.target.files)
      console.log('file3 =>' , this.$refs.file3.files[0].name)
      this.pic3 = this.$refs.file3.files[0].name
      const reader = new FileReader()
      reader.readAsDataURL(image)
      // console.log(`upload2 => ` , this.file2)
      reader.onload = (e) => {
        this.file3= e.target.result        
        // console.log('file', src)      
      }
    //   console.log('file', this.file)
    },


    }
  }
</script>