<template>
    <div>
        <form >
            <!--Вывод источника замера-->
            
            <h4>Источник замера</h4>
            <select v-model="selectedSource" @change="this.sourceID=$event.target.options.selectedIndex" >
                <option disabled value="">Выберите из списка</option>
                <option v-for="source in sources" :key="source.source_id" >
                    {{ source.source_name }}
                </option>
            </select>
             <!--Вывод состояние замера-->
             <h4>Состояние замера</h4>
            <select v-model="selectedState" @change="this.stateID=$event.target.options.selectedIndex">
                <option disabled value="">Выберите из списка</option>
                <option v-for="msrState in msrStates" :key="msrState.msr_state_id">
                    {{ msrState.msr_state_sname }}
                </option>
            </select>
            <!--Вывод типа замера-->
            <h4>Тип замера</h4>
            <select v-model="selectedType" @change="this.typeID=$event.target.options.selectedIndex">
                <option disabled value="">Выберите из списка</option>
                <option v-for="typeMsr in typeMsrs" :key="typeMsr.msr_type_id">
                {{ typeMsr.msr_type_sysname }}
                </option>
            </select>
            <!--Вывод  объектов-->
            <h4>Объекты</h4>
            <select v-model="selectedFacility" @change="this.facilityID=$event.target.options.selectedIndex">
                <option disabled value="">Выберите из списка</option>
                <option v-for="facility in facilities" :key="facility.facility_id">
                {{ facility.facility_name }}
                </option>
            </select>
            <h4>Тип значения</h4>
            <select v-model="selectedValueType" @change="this.valueTypeID=$event.target.options.selectedIndex">
                <option disabled value="">Выберите из списка</option>
                <option v-for="valueType in valueTypes" :key="valueType.value_type_id">
                {{ valueType.value_type_name }}
                </option>
            </select>
            <my-input v-model="dataMsr.msr_desc" @input="(dataMsr.msr_desc = $event.target.value)" type="text" placeholder="Примечание к замеру"/>
            <my-input  v-model="dataOfMsrValue.value" @input="(dataOfMsrValue.value = $event.target.value)" type="text" placeholder="Значение замера"/>
            <div class="addBtn">
                <my-button class="currenBtn" @click="addDatatoMsr">Сохранить</my-button>
            </div>
            

        </form>
    </div>
</template>

<script>
import axios from 'axios';

    export default {
        data(){
            return{
                sources:[],
                selectedSource: '',
                typeMsrs:[],
                selectedType:'',
                msrStates:[],
                selectedState: '',
                facilities:[],
                selectedFacility:'',
                valueTypes:[],
                selectedValueType:'',
                msrs:[],
                valueOfMsr: '',
                sourceID: 0,
                stateID: 0,
                typeID: 0,
                facilityID: 0,
                valueTypeID: 0,
                dataMsr:{
                    msr_desc: ""
                },
                dataOfMsrValue:{
                    value: ""
                }
                

            }
        },
        
        methods:{
            async getSources(){
                try{
                    const response = await axios.get('http://localhost:8081/source/all');
                    this.sources = response.data;
                    console.log(this.sources);
                }catch(e){
                    alert('Error')
                }
            },
            async getMsrTypes(){
                try{
                    const response = await axios.get('http://localhost:8081/msr_type/all');
                    this.typeMsrs = response.data;
                    console.log(this.typeMsrs);
                }catch(e){
                    alert('Error')
                }
            },
            async getMsrStates(){
                try{
                    const response = await axios.get('http://localhost:8081/msr_state/all');
                    this.msrStates = response.data;
                    console.log(this.msrStates);
                }catch(e){
                    alert('Error')
                }
            },
            async getfacilities(){
                try{
                    const response = await axios.get('http://localhost:8081/facility/all');
                    this.facilities = response.data;
                    console.log(this.facilities);
                    
                }catch(e){
                    alert('Error');
                }
            },
            async getMsrValueType(){
                try{
                    const response = await axios.get('http://localhost:8081/msr_value_type/all');
                    this.valueTypes = response.data;
                    console.log(this.valueTypes);
                    
                }catch(e){
                    alert('Error');
                }
            },
            async getMsrData(){
                try{
                    const response =  await axios.get('http://localhost:8081/msr/all');
                    this.msrs = response.data;
                    console.log(this.msrs);
                }catch(e){
                    alert('Error');
                }
            },
            async addDatatoMsr(){
                
                const response = await axios.post('http://localhost:8081/msr',this.dataMsr,{
                    params:{
                        typeID: this.typeID,
                        sourceID: this.sourceID,
                        stateID: this.stateID,
                        facilityID: this.facilityID
                    }
                });
                
                this.getMsrData();
                
                
                setTimeout(()=>{
                    this.addDataMsrValue();
                },100);
                
                
            },
            async addDataMsrValue(){
                const response = await axios.post('http://localhost:8081/msr_value',this.dataOfMsrValue,{
                    params:{
                        msrID: this.msrs[this.msrs.length-1].msr_id,
                        valueTypeID: this.valueTypeID
                    }
                });
            }

            

        },
        mounted(){
            this.getSources();
            this.getMsrTypes();
            this.getMsrStates();
            this.getfacilities();
            this.getMsrValueType()
            
            
        }
        
    }

</script>

<style  scoped>
form{
    display: flex;
    flex-direction: column;
}
select{
    width: auto;
    height: 30px;
    border: 1px solid black
}
.currenBtn{
    width: 150px;
}
.addBtn{
    text-align: center;
}
</style>