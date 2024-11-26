<template>
  <h1>Vehicles</h1>
  <div class='years'>
    <label for=''>Select Year </label>
    <select v-model="selectedYear" @change="getMakes">
      <option value="">Select Year</option>
      <option v-for="year in years" :key="year" :value="year">{{year}}</option>
    </select>
  </div>

  
  <div class='makes'>
    <label for=''>Select Make </label>
    <select v-model="selectedMakes" @change="getModels">
      <option value="">Select Make</option>
      <option v-for="make in makes" :key="make" :value="make.name">{{make.name}}</option>
    </select>
  </div>

  
  <div class='models'>
    <label for=''>Select Model </label>
    <select v-model="selectedModel">
      <option value="">Select Model</option>
      <option v-for="model in models" :key="model.model" :value="model.model">{{model.model}}</option>
    </select>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data(){
    return{
      years:[],
      makes:[],
      models:[],
      selectedYear:"",
      selectedMakes:"",
      selectedModel:"",
    }
  },
  created() {
    this.getYears();

  },
  methods:{
    async getYears() {
        const response = await axios.get('https://new.api.nexusautotransport.com/api/vehicles/years', {
          headers: {
            Accept: 'application/json',
            'Content-Type': 'application/json',
          },
        });
        this.years = response.data.data;
    },
    async getMakes(){
        this.selectedMakes="";
        this.selectedModel="";
        const response = await axios.get(`https://new.api.nexusautotransport.com/api/vehicles/makes?year=${this.selectedYear}`, {
          headers: {
            Accept: 'application/json',
            'Content-Type': 'application/json',
          },
        });
        //console.log(response.data.data);
        this.makes = response.data.data;
    },
    async getModels(){
        this.selectedModel="";
        const response = await axios.get(`https://new.api.nexusautotransport.com/api/vehicles/models?year=${this.selectedYear}&make=${this.selectedMakes}`, {
          headers: {
            Accept: 'application/json',
            'Content-Type': 'application/json',
          },
        });
        console.log(response.data.data);
        this.models = response.data.data;
    },
  }
  
}
</script>

