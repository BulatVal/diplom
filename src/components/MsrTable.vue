<template>
    <div class="filtres">
        <div class="filter">
            <my-input v-model="startDate" @input="( startDate = $event.target.value)" :value="startDate" type="date" placeholder="Введите первую дату"></my-input>
            <my-input v-model="endDate" @input="( endDate = $event.target.value)" :value="endDate" type="date" placeholder="Введите вторую дату"></my-input>
            <my-button class="currentbtn" @click="emptyFiltres">Убрать фильтры</my-button>
        </div>
    </div>
    <div class="tableMsr">
        <my-table :msrs="sortedByDate" @remove="removeMsr"></my-table>
        

    </div>
    <!-- <div>
        <dialog-window  v-model:show="dialogVisbleEdit">
            <EditMetricsForm disabled ></EditMetricsForm>
        </dialog-window>
    </div> -->
</template>

<script>
import axios from 'axios';
//import EditMetricsForm from './EditMetricsForm.vue';
    export default {
        // components:{
        //     EditMetricsForm
        // },
        
        data(){
            return{
                msrs:[],
                startDate: null,
                endDate: null,
                // dialogVisbleEdit:false
            }
        },
        methods:{
            // showDialogEdit(){
            //     this.dialogVisbleEdit = true;
            // },
            async loadMsrdata(){
                try{
                    const response = await axios.get('http://localhost:8081/msr_value/all');
                    this.msrs = response.data
                    console.log(this.msrs)
                }catch(e){
                    alert("Error")
                }
            },
            removeMsr(msr){
                this.msrs = this.msrs.filter(p => p.msr_value_id !== msr.msr_value_id);
                
                this.deleteMsrValueData(msr.msr_value_id);
                setTimeout(() => {
                    this.deleteMsrData(msr.msr_id.msr_id);
                }, 1000);
                
                
                
            },
            getId(msr){
                return msr.msr_id.msr_id;
            },
            //продолжить
            // async editMsrData(){
            //     let id = getId(msr)
            //     const response = await axios.put('http://localhost:8081/msr/'+ msrID +'/',this.dataMsr);
            // },
            async deleteMsrValueData(msr_value_id){
                try{
                    const response = await axios.delete('http://localhost:8081/msr_value/'+msr_value_id);
                    console.log('Запись успешно удалена');
                }catch(e){
                    alert('Error');
                }
            },
            async deleteMsrData(msr_id){
                try{
                    const response = await axios.delete('http://localhost:8081/msr/'+msr_id);
                    console.log('Запись успешно удалена');
                }catch(e){
                    alert('Error');
                }
                
            },
            emptyFiltres(){
                this.startDate = null;
                this.endDate =  null;
            }

        },
        mounted(){
           this.loadMsrdata();
           console.log(this.startDate);
           console.log(this.endDate);
        },
        computed:{
            sortedByDate(){
                if (this.startDate === null || this.endDate === null){
                    return this.msrs;
                }
                else{
                    return this.msrs.filter(p => p.msr_id.msr_date >= this.startDate && p.msr_id.msr_date <= this.endDate )
                }
                 
            }
        }
    }
</script>

<style  scoped>
.filtres{
    background-color: beige;
    border:2px solid black;
    margin-top:10px;
    border-radius:10px
}
.currentbtn{
    width: 150px;
}
.tableMsr {
    
    height: 500px;
    border: 2px solid black;
    overflow-x: scroll;
    overflow-y: scroll;
    border-radius: 10px;
    margin-right: 15px;
    margin-top: 10px;
}

</style>