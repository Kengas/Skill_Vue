<template>
<div class="text-center">



  <label>File1
          <input style="display:none;" id="file1" ref="file1" type="file" accept="image/*" @change="handleFileUpload1($event),submitFile1()">
        </label>
        <v-card height="10%" class="grey lighten-4 paddign">
          <img :src="file1" width="20%">
          <v-img v-if="this.img_copy1 == 'avatar.png' && this.test1 == '' "   
          :src= "`http://localhost:4500/${ this.img1} `"
          max-width="200"
          ></v-img>
          <v-img v-else    
          :src= "`http://localhost:4500/${ this.img1} `"
          max-width="200"
          ></v-img>
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
          ></v-text-field>
          
        </v-card-text>
        <v-divider class="mt-12"></v-divider>
        <v-card-actions>
          <v-btn text>
            Cancel
          </v-btn>
          <v-spacer></v-spacer>          
          <v-btn
            color="primary"
            text
            @click="update"
          >
            Update
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
    
    data: ()=>({
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
        img_copy1:'',
        img_copy2:'',
    }),
    watch: {
        std_id() {
            this.errorMessages = ''
        }
    },
    async created(){
        try{
          console.log('item=>', this.$route.query.std_id)
          let id = this.$route.query.std_id
          const res = await fetch('http://localhost:4500/edit?std_id=' + id)
          const data2 = await res.json()
          console.log('data=', data2.row.std_id)
          this.std_id = data2.row.std_id
          this.title = data2.row.title
          this.fname = data2.row.fname
          this.lname = data2.row.lname
          this.dep_id = data2.row.dep_id
          this.teacher_id = data2.row.teacher_id
          this.pass = data2.row.pass
          this.img1 = data2.row.img1
          this.img2 = data2.row.img2
          this.img3 = data2.row.img3   
          this.img_copy1 =  data2.row.img1
          this.img_copy2 =  data2.row.img1
          console.log(`img-copy => ` , this.img_copy1)
          console.log(`img-copy => ` , this.img_copy2)
        }catch(e){
          console.log(e.message)
        }
    },
    methods: {
        async update(){
          console.log('save')

            const std = {
                std_id: this.std_id,
                title: this.title,
                fname: this.fname,
                lname: this.lname,
                dep_id: this.dep_id,
                teacher_id: this.teacher_id,
                pass: this.pass,
                img1: this.test1,            
                img2: this.test2,            
                img3: this.test3,
                img_copy2:this.img_copy2     
            }
            console.log('user:', std)
            const res = await axios.post('http://localhost:4500/update', std)
            console.log('data=' , res.data.msg)
            this.msg = res.data.msg

            if(res.data.status == 1){

                console.log('save data')
                this.dialog = true
                setTimeout(()=>{
                    this.dialog = false
                    console.log('1')
                    this.$router.push('/list')
                    return
                },3000)

            }

        },
         // อัพโหลดข้อมูล
      async submitFile1 () {
       try{
          const formData = new FormData() // new ใช้สำหรับสร้าง object หรือ arryy ( ไม่แน่ใจว่า array ด้วยหรือป่าว )
          formData.append(`file` , this.$refs.file1.files[0] )
          console.log(`$refs => ` , this.$refs.file1.files )
          console.log(`formData => ` , this.$refs.file1.files[0]  )

          const res = await axios.post('http://localhost:4500/upload' , formData , {
            headers: {
              'content-type' : 'multipart/form-data'
            }
          })
          console.log(`ok1 => ` , res.data)
          console.log(`ok2 => ` , res.data.filesname)                    
          this.test1 = res.data.filesname
          this.file1.files[0].name = this.test1  
          // console.log(`test => ` , this.file1.files)
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
          // this.file1.files[0].name = this.test2
          // console.log(`test => ` , this.test2)
       }catch(e){
          console.log(e.message)
       }
      },
    handleFileUpload1 (e) {
      // this.file = this.$refs.file.files[0]
      const image = e.target.files[0]
      console.log('image1 =>' ,e.target.files)
      console.log('file1 =>' , this.$refs.file1.files[0].name)
      this.pic1 = this.$refs.file1.files[0].name
      const reader = new FileReader()
      reader.readAsDataURL(image)
      reader.onload = (e) => {
        this.file1 = e.target.result
        console.log('file', this.src)
      }
    //   console.log('file', this.file)
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