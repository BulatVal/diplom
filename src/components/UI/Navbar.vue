<template>
    <div class="navbar">
        <my-button class="currentButton" @click="$emit('showDialog')">
            Добавить запись 
        </my-button>
        
            <my-button class="currentButtonexit" @click="$router.push('/')">
                Выйти
            </my-button>
        
        
    </div>
    
</template>

<script>
    export default {
        name: 'nav-bar',
        
        
    }
</script>

<style  scoped>
.navbar{
    height: 50px;
    background-color: #FF9966;
    box-shadow: 2px 2px 4px gray ;
    display: flex;
    
    padding: 0 15px;
}
.currentButton{
    width: 150px;
    
}
.currentButtonexit{
    width: 150px;
    margin-left: 1250px;
}



</style>