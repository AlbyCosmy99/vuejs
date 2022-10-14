<template>
  <v-container>
      <div style="background-color:white">
        <h1>INSERT NEW USER</h1><hr>
        <input v-model="name" type="text" placeholder="insert name">
        <br>
        <hr>
        <input v-model="surname" type="text" placeholder="insert surname">
        <hr>
        <button style="color:white; background-color:green" @click="post">PRESS TO INSERT</button>
      </div>
      <br>
      <div>
        <button style='color:white; background-color: purple;' @click="show_users">{{show_hide}} USERS LIST</button>
        <br>
        <div v-if="on_show_users">
          <div v-for="user in users" :key="user.id">
            <h3>USER</h3>
            NAME: {{user.name}}
            <br>
            SURNAME: {{user.surname}}
          </div>

        </div>
      </div>
  </v-container>
</template>
<script>
  import axios from 'axios'
  export default {
    name: 'HelloWorld',

    data: () => ({
      message : '',
     name : '',
     surname : '',
     users : [],
     on_show_users : false,
     show_hide : 'SHOW'
    }),
    mounted(){
      this.get();
    },
    methods:{
      get(){
        axios.get('http://192.168.1.8:8000/users/').then(response=>{
          this.users = response.data;
        });
      },
      post(){
        if(this.name!='' && this.surname!=''){
          axios.post('http://192.168.1.8:8000/users/',{
            name : this.name,
            surname :this.surname
          });
          this.name = '';
          this.surname = '';
        }
        else{
          alert('Something got wrong. Insert both name and surname and try again.')
        }
      },
      show_users(){
        this.get();
        this.on_show_users = !this.on_show_users;
        if(this.show_hide=='SHOW'){
          this.show_hide = 'HIDE';
        }
        else{
          this.show_hide = 'SHOW';
        }
      }
    }
  }
</script>
