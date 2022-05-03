<template>
  <div>

    <v-btn
      class="ma-2"
      color="secondary"
      @click="logout"
    >
      Logout
    </v-btn>

    <h1>Student List</h1>
    <v-btn
      class="mx-2"
      fab
      dark
      color="indigo"
      @click="adddata"
    >
      <v-icon dark>
        mdi-plus
      </v-icon>
    </v-btn>

 <v-dialog
      v-model="dialog"
      persistent
      max-width="350"
    >
      <v-card>
        <v-card-title class="text-h5">
          Do you want to delete this?
        </v-card-title>      
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="dialog = false , stat = 0"
          >
            NO
          </v-btn>
          <v-btn
            color="green darken-1"
            text
            @click="dialog = false , stat = 1 , test(std_code)"
          >
            YES
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <ul>
      <li v-for="st in student" :key="st.code">
        <v-avatar size="100">
        <v-img        
          :src= "`http://localhost:4500/${ st.img1} `"
        ></v-img>
        </v-avatar>
        
        <span>{{ st.std_id }}</span>
        <span>{{ st.lname }}</span>        
        <v-btn @click="edit(st.std_id)">Edit</v-btn>
        <span><v-icon 
        small 
        class="mr-2" 
        @click="edit(st.std_id)"
        >
          mdi-pencil
          </v-icon></span>
          <span><v-icon 
          samll 
          class="mr-2"
           @click="del(st.std_id)"
           >
            mdi-delete</v-icon></span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data: () => ({    
      student: [
        { std_id: '001', lname: 'Name1' ,img:'avatar.jpg'},
      ],
      dialog: false,
      stat: 0,
      std_code: '',
  }),
  mounted() {
      // if(sessionStorage.getItem('status') == 1){
        
      // }else{
      //   this.$router.push('/login')
      // }
    },
  created(){
      this.list()
  },
  methods: {
      adddata(){
          console.log('adddata')
          this.$router.push('register')
      },
      edit(item){
        console.log('edit=',item)
      },
     async edit(item){
          console.log('edit',item)
          this.$router.push('/edit?std_id=' + item)   
      },
     async del(item){
        try{
          this.dialog = true
          //  console.log(`dialog =>` , this.dialog)
          //  console.log(`stat =>` , this.stat)          
                if(this.dialog == true){
                  console.log(`dialog =>` , this.dialog)
                    if(this.stat == 1){
                      console.log(`remove =>` , this.stat)
                      // console.log('del',item)
                      // const res = await fetch('http://localhost:4500/del?std_id=' + item)
                      // const data2 = await res.json()          
                      // console.log('data_del=',data2)
                      // setTimeout(()=>{
                      //   this.$router.push('/list')
                      // },500)
                    }else{
                      console.log(`no remove =>` , this.stat)
                    }
                }                
                this.std_code = item
                console.log('std_id => ' , item)    
          }catch(e){
            console.log(e.message)
          }
      },
 async  test(item){
        console.log(`Hello Test => ` , item)   
        console.log('del',item)
        const res = await fetch('http://localhost:4500/del?std_id=' + item)
        const data2 = await res.json()          
        console.log('data_del=',data2)
        setTimeout(()=>{
          this.$router.push('/list')
        },500)             
      },
      async list(){
          const res = await fetch('http://localhost:4500/list')
          const data2 = await res.json()
          this.student = data2.datas
          console.log('data=',data2.datas)
          // console.log('member =>', member)
      },
      logout(){
        this.$router.push('/logout')
      }
  },
}
</script>