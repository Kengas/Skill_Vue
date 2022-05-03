<template>
  <div class="drop">
    
    <v-col class="d-flex" cols="12" sm="6">
        <v-select v-model="dep" :items="department" label="เลือกแผนกของท่าน" dense ></v-select>      

      </v-col>

      <v-btn depressed color="primary">
        {{ username }} ({{ user_level }}) <br> Login
      </v-btn>

      <v-btn class="ma-2" color="red" @click="logout" >
        Logout
      </v-btn>

      <v-btn color="warning" @click="edit" >
        EDIT
      </v-btn>

      <h1>{{ dep }}</h1>

    <!-- <h1>ครูที่ปรึกษา {{ this.advisor_first }} &nbsp; {{ this.advisor_last }} </h1> -->
    <v-card class="ma-2 card1" elevation="0">
      <h1>เช็คชื่อ</h1>
      <v-sheet id="scrolling-techniques-7" class="overflow-y-auto" max-height="350">
        <v-simple-table>
          <template #default>
            <thead>
              <tr>
                <th class="text-left">
                  รหัสนักศึกษา
                </th>
                <th class="text-left">
                  ชื่อ
                </th>
                <th class="text-left">
                  นามสกุล
                </th>
                <th class="text-left">
                  การเข้าร่วม
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for=" (item , index) in users_student" :key="item.student_id">
                <td>{{ item.student_id }}</td>
                <td>{{ item.user_first }}</td>
                <td>{{ item.user_last }}</td>
                <p>{{ index }}</p>
                <td>
                  <v-radio-group v-model="status[index]" row>
                    
                    <v-radio
                      label="ขาด"
                      value="fail"
                      color="red"                      
                    />

                    <v-radio
                      label="ลา"
                      value="sick"
                      color="orange"
                    />

                    <v-radio
                      label="มา"
                      value="come"
                      color="success"
                    />

                  </v-radio-group>
                </td>                              

              </tr>
            </tbody>            
          </template>
        </v-simple-table>
      </v-sheet>
      <br>
      <div class="text-center">
        <v-btn
          color="blue darken-3"
          rounded
          class="btn1"
          dark
          elevation="0"
          @click="onSave"
        >
          SAVE
        </v-btn>
      </div>
      <br>
    </v-card>
    <h4>Status : {{ status }}</h4>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  layout: 'Theme1',
  data  () {
    return {
      users_student: [],
      status: [],
      department:[
        'เทคโนโลยีสารสนเทศ' ,
        'คอมพิวเตอร์ธุรกิจ' ,
        'อิเล็กทรอนิกส์' ,
      ],
      dep:'',      
      advisor_first: '',
      advisor_last: '',
      student_user_id: '',
      check_id : '',
      info: [],
      username: '',
      user_level: '',
      teacher_check: '',
      advisor_check: '',
      teacher_id: '',
    }
  },
  mounted() {
     this.username = sessionStorage.getItem('username')
     this.user_level = sessionStorage.getItem('user_level')
     this.teacher_id = sessionStorage.getItem('user_id')
     if(sessionStorage.getItem('user_level') == 'ge_teach'){
           this.teacher_check = sessionStorage.getItem('username')
      }
      if(sessionStorage.getItem('user_level') == 'advisor'){
           this.advisor_check = sessionStorage.getItem('username')
      }
  },
  created () {
    this.showData()
  },
  methods: {
    async showData () {      
      const res = await fetch('http://localhost:4500/list_std')
      const data = await res.json()
      this.users_student = data.datas
      console.log(this.users_student)
      this.advisor_first =  this.users_student[0].advisor_first
      this.advisor_last =  this.users_student[0].advisor_last
      // this.teacher_id =  this.users_student[0].teacher_id
      // console.log(`User-Student => ` , this.users_student.datas)
      // console.log(`advisor_first => ` , this.users_student)
      // this.department = this.users_student.major_name
      // console.log(`department => ` , this.users_student.major_name)
    },
    async onSave () {
      try{

        // const info = {
        //   student_id : this.users_student[0].student_id,
        //   user_first : this.users_student[0].user_first,
        //   user_last : this.users_student[0].user_last,
        //   status : this.status[0],
        //   student_user_id : this.users_student[0].student_user_id,
        // }

        let index = 0
        var info = []

        for(index = 0; index < this.users_student.length ; index++){
             info[index] = [
                      {student_id : this.users_student[index].student_id},
                      {user_first : this.users_student[index].user_first},
                      {user_last : this.users_student[index].user_last},
                      {status : this.status[index]},
                      {student_user_id : this.users_student[index].student_user_id},
                      {teacher_check : this.teacher_check},
                      {advisor_check : this.advisor_check},
                      {teacher_id : this.teacher_id},
                    ]
        }

        // for(let i = 0 ; i < 5; i++){
        //   console.log(` i => ${i} `)
        // }

        

       console.log(`users_student-length => `, this.users_student.length)
       console.log(`info => `, info[0])
      //  console.log(`info => `, info)

        
        const res = await axios.post('http://localhost:4500/save' , info )

        // console.log(`save_group =>` , this.users_student[0].group_id)
        // console.log(`save_id =>` , this.users_student[0].id)
      }catch(e){
        console.log(e.message)
      }
    },
    edit(item){
        console.log('edit=',item)
        this.$router.push('/edit_project')
    },
    logout(){
        this.$router.push('/logout')
    },
  },
}
</script>
<style>
.card1{
  padding: 14px;
}
.btn1{
  width: 140px;
}
.drop{
  margin-top: 30px;
}
</style>