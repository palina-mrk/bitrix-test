<template>
  <div id="app">
    <h1>Список компаний:</h1>
    <ul v-for="company in companies" :key="company.ID">
      <li>{{ company.TITLE }} (ID: {{ company.ID }})</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      companies: []
    };
  },
  mounted() {
    this.fetchCompanies();
  },
  methods: {
    async fetchCompanies() {
      try {
        const response = await axios.get('https://your-domain.bitrix24.ru/rest/crm.company.list.json', {
          params: {
            auth: 'your-auth-token-here',
            select: ['*'], 
            order: { ID: 'ASC' },
            limit: 10000 
          }
        });
        
        if(response.data.result && Array.isArray(response.data.result)) {
          this.companies = response.data.result;
        } else {
          console.error("Ошибка обработки результата:", response);
        }
      } catch(error) {
        console.error("Ошибка при получении списка компаний:", error.message);
      }
    }
  }
}
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}

ul {
  list-style-type: none;
  padding-left: 0;
}

li {
  background-color: #f8f8ff;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 5px;
  box-shadow: 0 2px 4px rgba(0,0,0,.1);
}
</style>
