<template>
  <div class="wrapper">
    <div class="head">
      <h1>Rick and Morty</h1>
      <div class="filters">
        <input v-model="filters.name" placeholder="Фильтрация по имени">
        <select v-model="filters.status">
          <option value="">Все статусы</option>
          <option value="Alive">Alive</option>
          <option value="Dead">Dead</option>
          <option value="unknown">Unknown</option>
        </select>
        <button @click="applyFilters">Принять</button>
      </div>
    </div>
    <div class="main">
      <CharCard 
        v-for="card in cards" 
        :key="card.id" 
        :name="card.name" 
        :status="card.status" 
        :species="card.species" 
        :image="card.image" 
        :origin="card.origin.name" 
        :location="card.location.name"
      />
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
</template>

<script>
import CharCard from './components/CharCard.vue';
import { ref, onMounted, reactive } from 'vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    CharCard,
  },
  setup() {
    const cards = ref([]);
    const currentPage = ref(1);
    const totalPages = ref(1);
    const filters = reactive({
      name: '',
      status: ''
    });

    const fetchData = async (page = 1) => {
      try {
        const response = await axios.get('https://rickandmortyapi.com/api/character/', {
          params: {
            page,
            name: filters.name,
            status: filters.status
          }
        });
        if (response.data && response.data.results && response.data.info) {
          cards.value = response.data.results;
          totalPages.value = response.data.info.pages;
          console.log(cards.value);
        } else {
          console.error('Unexpected response structure:', response.data);
        }
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    const nextPage = () => {
      if (currentPage.value < totalPages.value) {
        currentPage.value += 1;
        fetchData(currentPage.value);
      }
    };

    const prevPage = () => {
      if (currentPage.value > 1) {
        currentPage.value -= 1;
        fetchData(currentPage.value);
      }
    };

    const applyFilters = () => {
      currentPage.value = 1; // Reset to the first page
      fetchData();
    };

    onMounted(() => {
      fetchData();
    });

    return {
      cards,
      currentPage,
      totalPages,
      nextPage,
      prevPage,
      filters,
      applyFilters
    };
  },
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
}
* {
  margin: 0;
  padding: 0;
}
.wrapper {
  display: flex;
  flex-direction: column;
}
.head {
  text-align: center;
  padding: 3%;
  font-size: 30px;
}
.filters {
  display: flex;
  justify-content: center;
  margin-top: 10px;
  margin-bottom: 20px;
}
.filters input,
.filters select {
  margin: 0 10px;
  padding: 5px;
}
.filters button {
  padding: 5px 10px;
  cursor: pointer;
}
.main {
  display: flex;
  flex-wrap: wrap;
  background: rgb(53, 52, 62);
  color: whitesmoke;
  padding: 5%;
}
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}
.pagination button {
  margin: 0 5px;
  padding: 5px 10px;
  cursor: pointer;
}
</style>
