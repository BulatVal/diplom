<template>
    <table cellspacing="0" cellpadding="0">
        <tbody>
            <tr>
                
            <th colspan="4">Объект</th>
            <th colspan="5">Исходный замер</th>
            <!-- <th colspan="6">Отбраковка</th> -->
           </tr>
           <tr>
            <td colspan="2">?</td>
            <td>Источник</td>
            <td>Тип НПО</td>
            <td>НПО</td>
            <td>Дата</td>
            <td>Замер</td>
            <td>Единицы измерения</td>
            <td>Примечание к замерам</td> 
            <!-- <td>Qж, зам</td>
            <td>Qж, ож</td>
            <td>Qж, тех</td>
            <td>Qж, реж</td>
            <td>Результат</td>
            <td>Дата</td>      -->
           </tr>
           <tr v-for="msr in msrs" :key="msr.msr_value_id" >
            <td><my-button @click="$emit('remove',msr)">х</my-button></td>
            <td>
                <div v-if="msr.msr_id.source_id.source_name!== 'two'">
                    -
                </div>
                <div v-else><my-button @click="this.selectedMsrID=showDialogEdit(msr)"  >↻</my-button></div>
            </td>
            <td>{{ msr.msr_id.source_id.source_name }}</td>
            <td>{{ msr.msr_id.facility_id.facility_type_id.facility_type_name }}</td>
            <td>{{ msr.msr_id.facility_id.facility_name }}</td>
            <td>{{ msr.msr_id.msr_date }}</td>
            <td>{{ msr.value}}</td>
            <td>{{ msr.msr_value_type_id.value_type_name}}</td>
            <td>{{ msr.msr_id.msr_desc }}</td>
            <!-- <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>              -->
           </tr>     
        </tbody>
           
    </table>
    <div>
        <dialog-window  v-model:show="dialogVisbleEdit">
            <EditMetricsForm :selected-msr="selectedMsrID" ></EditMetricsForm>
        </dialog-window>
    </div>
</template>

<script>

import EditMetricsForm from '../EditMetricsForm.vue';
    export default {
        components:{
            EditMetricsForm
        },
        data(){
            return{
                selectedMsrID: 0,
                dialogVisbleEdit:false
            }
        },
        
        
        name: 'my-table',
        props:{
            msrs:{
                type: Array,
                required: true
            }
                
        },
        methods:{
            showDialogEdit(msr){
                this.dialogVisbleEdit = true;
                return msr.msr_id.msr_id
            }
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
    background-color: beige;
    font-size: 13pt;
}
td{
    border: 1px solid black;
    text-align: center;
}
</style>