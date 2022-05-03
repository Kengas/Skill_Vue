<template>
  <v-data-table
    :headers="headers"
    :items="students"
    sort-by="calories"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>My CRUD</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>

    <!-- ===== Dialog_r-start ===== -->
        <v-dialog
      v-model="dialog_r"
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
            @click="dialog_r = false"
          >
            I accept
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- ===== Dialog_r-end ===== -->

        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.std_id"
                      label="Student ID"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.fname"
                      label="First Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.lname"
                      label="Last Name"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.teacher_id"
                      label="Teacher ID"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <input id="file" ref="file" type="file" @change="handleFileUpload($event),submitFile()">
                    <img :src="file" width="100%">

                    <button @click="submitFile()">
                      Submit
                    </button>

                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
import axios from 'axios'
  export default {
    data: () => ({
      test:'',
      del: '',
      dialog: false,
      dialog_r: false,
      dialogDelete: false,
      headers: [
        { text: 'รหัสนักศึกษา', align: 'start', sortable: false, value: 'std_id'},
        { text: 'คำนำหน้า', value: 'title' },
        { text: 'ชื่อ', value: 'fname' },
        { text: 'นามสกุล', value: 'lname' },
        { text: 'รหัสครูที่ปรึกษา', value: 'teacher_id' },
        { text: 'รูปภาพ', value: 'img', sortable: false },
        { text: 'Action', value: 'actions', sortable: false },
      ],
      students: [],
      editedIndex: -1,
      editedItem: {
        std_id: '',
        title: '',
        fname: '',
        lname: '',        
        dep_id: '',
        teacher_id: '',
        pass: '',
        img: '',
        msg: '',
      },
      deFualtItem: {
        std_id: '',
        title: '',
        fname: '',
        lname: '',        
        dep_id: '',
        teacher_id: '',
        pass: '',
        img: '',
        msg: '',
      },
      edit1: {
        std_id: '',
        title: '',
        fname: '',
        lname: '',        
        dep_id: '',
        teacher_id: '',
        pass: '',
        img: '',
        msg: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },

    created () {
      this.initialize()
      this.list()
    },

    methods: {
    async  list(){
          const res = await fetch('http://localhost:4500/list')
          const data2 = await res.json()
          this.students = data2.datas
          // console.log('data=',data2.datas)
      },
      initialize () {
        this.students = [
          {
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
          },
          {
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
          },
        ]
      },

      editItem (item) {
        console.log('item => ' , item)
        console.log('indexOf => ' , this.students.indexOf(item))
        this.editedIndex = this.students.indexOf(item)
        this.editedItem = Object.assign({}, item)
        console.log('object= ' , Object.assign({}, item))
        this.dialog = true
      },

      deleteItem (item) {       
        this.dialogDelete = true      
        this.editedIndex = this.students.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.del = this.editedItem.std_id   
        // console.log('del',item)
        // console.log('del-edited =>', this.editedItem.std_id)             
        // console.log('data_del=',data2) 
        // console.log('del-item',item.std_id)          
      },

     async deleteItemConfirm() {
        // this.students.splice(this.editedIndex, 1)
        console.log('del_ok', this.del)
        const res = await fetch('http://localhost:4500/del?std_id=' + this.del)
        const data2 = await res.json()
        console.log('data_del=' , data2) 
        this.closeDelete()
      },
      
      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },

     async save () {
        if (this.editedIndex > -1) {
              try{
                //   this.edit1 = Object.assign(this.students[this.editedIndex], this.editedItem)
               //   console.log('edit-1=>' , this.edit1)
              
              const res = await axios.post('http://localhost:4500/update' , this.editedItem)
              console.log('data=' , res.data.msg)                        
              this.msg = res.data.msg
              this.editedItem.img = res.data.filesname
              console.log('file=' , editedItem.img)
              console.log(`msg => ` ,this.msg)
                  if(res.data.status == 1){
                      console.log('save data')
                      this.dialog_r = true
                      setTimeout(()=>{
                          this.dialog_r = false
                          console.log('1')
                          // this.$router.push('/list')
                          return
                      },3000)
                  } 
              }catch(e){
                console.log(e.message)
              }
            }else {

              // students ข้อมูลทั้งหมด ซึ่งจะถูกแบ่งออกเป็นอ็อปเจ็ก
              // editedItem อ็อปเจ็กที่เก็บค่าเอาไว้ข้างใน
              const res = await axios.post('http://localhost:4500/save', this.editedItem)
              this.students.push(this.editedItem)              
              this.msg = res.data.msg
              console.log('data=', res.data.msg)
              console.log('editedItem => ' , this.editedItem)
              console.log('students => ' , this.students)
              console.log('push')

                if(res.data.status == 0){
                  console.log('ID has exists')
                  this.dialog = true
                  setTimeout(()=>{
                    this.dialog = false
                    console.log('0')
                    return
                  },2000)
                }else{
                  console.log('save data')
                  this.dialog = true
                  setTimeout(()=>{
                    this.dialog = false
                    console.log('1')
                    return
                  },2000)                  
                }
              
            }
        this.close()
      },
      // อัพโหลดข้อมูล
      async submitFile () {
       try{
          const formData = new FormData() // new ใช้สำหรับสร้าง object หรือ arryy ( ไม่แน่ใจว่า array ด้วยหรือป่าว )
          formData.append(`file` , this.$refs.file.files[0] )
          console.log(`$refs => ` , this.$refs.file.files[0] )
          console.log(`formData => ` , this.$refs.file.files[0]  )

          const res = await axios.post('http://localhost:4500/upload' , formData , {
            headers: {
              'content-type' : 'multipart/form-data'
            }
          })
          console.log(`ok1 => ` , res.data)
          console.log(`ok2 => ` , res.data.filesname)
          this.editedItem.img = res.data.filesname
       }catch(e){
          console.log(e.message)
       }
      },
async  handleFileUpload(e){
          try{
            const image = e.target.files[0]
            console.log( `target => ` , e.target.files[0])
            const render = new FileReader()
            render.readAsDataURL(image)
            render.onload = (e) => {
              this.file = e.target.result
            }

          }catch(e){
            console.log(e.message)
          }
      },
    },
  }
</script>