<template>
    <div>
    <form @submit.prevent="submit">

        <v-text-field
          v-model="username"          
          label="Username"
        ></v-text-field>

        <v-text-field
          v-model="password"          
          label="Password"
          type="password"
        ></v-text-field>


      <v-btn
        class="mr-4"
        type="submit"
        
      >
        submit
      </v-btn>
      <v-btn @click="clear">
        clear
      </v-btn>
    </form>

    </div>
</template>
<script>
import axios from 'axios'
  export default {
    data: () => ({
      username: '',
      password: '',
    }),
    methods: {
     async submit () {
        
        try{
          const member = {
            username: this.username,
            password: this.password,            
          }
          const res = await axios.post('http://localhost:9000/api/signin' , member)    
          // console.log(`user => ` , this.username)
          // console.log(`pass => ` , this.password)        
          // console.log(`member => ` , member)        
          // console.log(`res => ` , res.data.status)

          if(res.data.token){
            // sessionStorage.setItem("username" ,  res.data.val.username)
            // sessionStorage.setItem("password" ,  res.data.val.password)
            // sessionStorage.setItem("status" ,  res.data.status)  
            // sessionStorage.setItem("user_id" ,  res.data.val.user_id)
            // sessionStorage.setItem("user_level" ,  res.data.val.user_level)
            localStorage.setItem("token" , res.data.token)
            // // console.log(`session-username => ` , res.data.val.user_id)
            // // console.log(`session-password => ` , res.data.val.password)
            // this.$router.push('/check')
            console.log('complete => ' , res.data.ok)
          }else{                  
            console.log('failed => ' , res.data.ok)
          }
        }catch(e){
          console.log(e.message)
        }

      },
      clear () {
        this.username = ''
        this.password = ''
        // this.$refs.observer.reset()
      },
    },

  }
</script>
