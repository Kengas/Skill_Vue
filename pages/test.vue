<template>

 <div class="text-center">
    <v-layout row wrap>
            <v-flex xs4></v-flex>
            <v-flex xs4 class="text-xs-center"
                @click="$refs.img.click()"
                style="cursor: pointer;"
              >
                <input
                  label="picture"
                  type="file"
                  style="display:none;"
                  accept="image/*"
                  multiple
                  @change="upload_img($event)"
                  ref="img"
                >
                <v-card  width="40%" height="40%" class="grey lighten-4 paddign" >
                  <img :src="this.img" width="100%">
                  <v-card-actions style="font-size:100%">
                    <span><i class="fas fa-image fa-2x"></i></span>
                    <v-spacer></v-spacer>
                    <span>รูปภาพ</span>
                  </v-card-actions>
                </v-card>
              </v-flex>
        </v-layout>

    <v-dialog
      v-model="dialog"
      width="500"
    >
      <v-card>
        <v-card-title class="text-h5 success ">
          แจ้งเตือน...
        </v-card-title>

        <v-card-text>
          ข้อมูลซ้ำ =>{{ msg }}

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
            ref="id22"
            v-model="id"
            :rules="[() => !!id || 'This field is required test']"
            label="รหัสนักศึกษา"
            placeholder="6430901001"
          ></v-text-field>
          <v-text-field
            ref="title"
            v-model="title"
            label="คำนำหน้า"
            placeholder="นาย"
          ></v-text-field>
          <v-text-field
            ref="fname"
            v-model="fname"
            label="ชื่อ"
            placeholder="อลงกรณ์"
          ></v-text-field>
            <v-text-field
            ref="lname"
            v-model="lname"
            label="สกุล"
            placeholder="John Doe"
          ></v-text-field>
          <v-text-field
            ref="pass"
            v-model="pass"
            label="รหัสผ่าน"
            placeholder="6430901001"
          ></v-text-field>
          <v-text-field
            ref="teacher_id"
            v-model="teacher_id"
            label="รหัสครูที่ปรึกษา"
            placeholder="6430901001"
          ></v-text-field>
          <v-text-field
            ref="dep_id"
            v-model="dep_id"
            label="รหัสแผนก"
            placeholder="123456"
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
import { setTimeout, clearInterval } from 'timers';
export default {

  data: () => ({
  id: '',
  fname: '',
  title: '',
  lname: '',
  dep_id: '',
  teacher_id: '',
  pass: '',
  img: 'avartar.jpg',
  dialog: false,
  msg: '',
  status: ''
  }),
  methods: {
    async submit(){
      console.log('save')
      const std = {
        id: this.id,
        title: this.title,
        fname: this.fname,
        lname: this.lname,
        dep_id: this.dep_id,
        pass: this.pass,
        teacher_id: this.teacher,
        img: this.img,

      }
      console.log('user:', std)
      const res = await axios.post('http://localhost:9000/save', std)
      console.log('data=',res.data.msg)
      this.msg = res.data.msg
      if(res.data.status == 0){
        console.log('มีข้อมูลอยู่แล้ว')
        this.dialog = true

       const fa = setInterval(() => {
          this.dialog = false
        //  this.$router.push('/register/')
          console.log('0')


        }, 4000);
// clearInterval(fa)


      }else if (res.data.status == 1) {
        this.dialog = true
        res.data.status = 2
      const tr =  setInterval(() => {
          this.dialog = false
           console.log('1')
          // this.$router.push('/register/')
        }, 4000);
// clearInterval(tr)
      }


    },
  upload_img(e){
            const image = e.target.files[0];
            console.log('img=',this.img);
            const reader = new FileReader();
            reader.readAsDataURL(image);
            reader.onload = e =>{
              this.img=e.target.result;
               console.log('name=>',this.img);
            };
          },
  },
}
</script>