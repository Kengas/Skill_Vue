<template>
  <div>
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
    <ul>
      <li v-for="st in student" :key="st.code">
        <span>{{ st.std_id }}</span>
        <span>{{ st.lname }}</span>
        <span>{{ st.img }}</span>
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
  }),
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
      del(item){
        console.log('del=',item)
      },
     async edit(item){
          console.log('edit',item)
          this.$router.push('/edit?std_id=' + item)           
      },
     async del(item){
          console.log('del',item)
          this.$router.push('/del?std_id=' + item)              
      },
      async list(){
          const res = await fetch('http://localhost:4500/list')
          const data2 = await res.json()
          this.student = data2.datas
          console.log('data=',data2.datas)
      }
  },
}
</script>