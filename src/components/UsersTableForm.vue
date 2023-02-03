<template>
    <div class="userTable" v-if="showUsers" @click="hideDialog">
        <my-button class="currentButton" @click="showDialog" >Добаить запись</my-button>
        
        <div class="scrollUser">
            <table cellspacing="0" cellpadding="0">
                <tbody>
                    <tr>
                        <th>Фамилия</th>   
                        <th >Имя</th>
                        <th >Отчество</th>
                        <th >Логин</th>
                        <th >Пароль</th>
                        <th>Роль</th>
                    </tr>
                    <tr v-for="user in users" :key="user.user_id" >
                        <td>{{ user.user_secondname}}</td>
                        <td>{{ user.user_firstname }} </td>
                        <td>{{ user.user_thirdname }}</td>
                        <td>{{user.user_login }}</td>
                        <td>{{ user.user_password}}</td>
                        <td>{{ user.user_role_id.role_name}}</td> 
                    </tr>     
                </tbody>
            </table>
        </div>
        
        <dialog-window v-model:show="dialogVisble">
                <div class="exitBtn">
                    <my-button @click="closeDialog">х</my-button>
                </div>
                <my-input v-model="dataUser.user_secondname" @input="(dataUser.user_secondname = $event.target.value)" type="text" placeholder="Фамилия"></my-input>
                <my-input v-model="dataUser.user_firstname" @input="(dataUser.user_firstname = $event.target.value)" type="text" placeholder="Имя"></my-input>
                <my-input v-model="dataUser.user_thirdname" @input="(dataUser.user_thirdname = $event.target.value)" type="text" placeholder="Отчество"></my-input>
                <my-input v-model="dataUser.user_login" @input="(dataUser.user_login = $event.target.value)" type="text" placeholder="Логин"></my-input>
                <my-input v-model="dataUser.user_password" @input="(dataUser.user_password = $event.target.value)" type="text" placeholder="Пароль"></my-input>
                <div class="saveBtn">
                    <my-button class="currentButton" @click="addData">Сохранить</my-button>
                </div>
                
            
            
        </dialog-window>
    </div>
</template>

<script>
import axios from 'axios';
    export default {
        data(){
            return{
                users:[],
                dialogVisble: false,
                dataUser:{
                    user_firstname:'',
                    user_secondname:'',
                    user_thirdname:'',
                    user_login:'',
                    user_password:''

                },
                
                checkLogin:[]
                
                
            }
        },
        props:{
            showUsers :{
                type:Boolean,
                default:false
            }
        },
        methods:{
            hideDialog(){
                this.$emit('update:show',true);
            },
            closeDialog(){
                this.dialogVisble =false;
            },
            showDialog(){
                this.dialogVisble =true;
            },
            addData(){
                this.checkLogin = this.users.filter(p => p.user_login === this.dataUser.user_login)
                if (this.dataUser.user_firstname === '' || 
                    this.dataUser.user_secondname === '' ||
                    this.dataUser.user_thirdname === '' || 
                    this.dataUser.user_login === '' ||
                    this.dataUser.user_password === ''){
                        alert('Заполните все поля')
                    }
                else if(this.dataUser.user_password.length < 8){
                        alert('Пароль должен быть не менее 8 символов');
                    }
                else if (this.checkLogin.length > 0 ){
                    alert('Пользователь с такми логином уже сущетсвует');
                    
                }else{
                    this.addUserToDB();
                    this.closeDialog();
                    this.dataUser.user_firstname ='';
                    this.dataUser.user_secondname ='';
                    this.dataUser.user_thirdname = '';
                    this.dataUser.user_login = '';
                    this.dataUser.user_password = '';
                }
            },
            async getAllUsers(){
                try{
                    const response = await axios.get("http://localhost:8081/users/all");
                    this.users = response.data;
                    
                }catch(e){
                    alert('Error')
                }
                
            },
            async addUserToDB(){
                try{
                    const response = await axios.post("http://localhost:8081/users",this.dataUser,{
                        params:{
                            roleID: 2
                        }

                    });
                    alert('Пользователь добавлен');
                }catch(e){
                    alert('Error');
                }
            }
        },
        mounted(){
            this.getAllUsers();
        }
    }
</script>

<style  scoped>
table{
    width: 100%;
    font-family: "Lucida Sans Unicode", "Lucida Grande", Sans-Serif;
   font-size: 14px;
   border-radius: 15px;
   border-spacing: 0;
   text-align: center;
  
    
}
.saveBtn{
    text-align: center;
}
.exitBtn{
    text-align: right;
}
.currentButton{
    width: 150px;
}
.userTable{
    margin-top: 30px;
}
form{
    display: flex;
    flex-direction: column;
}
.scrollUser{
    height: 500px;
    border: 2px solid black;
    overflow-x: scroll;
    overflow-y: scroll;
    border-radius: 10px;
    margin-right: 15px;
    margin-top: 10px;
}

table tr:last-child td:first-child{
   border-radius: 0 0 0 10px;
}
table tr:last-child td:last-child{
   border-radius: 0 0 10px 0;
}  

table tr:first-child th:last-child{
   border-radius: 0 10px 0 0;
}  
table tr:first-child th:first-child{
   border-radius: 10px 0 0 0;
}  

th{
    border: 1px solid black;
    margin: 0px;
    text-align: center;
    font-size: 13pt;
    background-color: beige;
}
td{
    border: 1px solid black;
    text-align: center;
}
</style>