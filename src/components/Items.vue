<template>
    <div>
      <ul>
        <li v-for="item in filteredItems" :key="item.id">
          <form method="PUT" :action="`http://127.0.0.1:8000/api/item/${item.id}`">
            <h1>
              {{ item.name }}
              <input type="checkbox" v-model="toggle" true-value="1" false-value="false" />
            </h1>
  
            <span v-if="item.description">{{ item.description }}</span>
            <br>
            <input type="submit" class="button update" value="update">
          </form>
            <button type="submit" class="button delete" @click="deleteItem(item.id)">Delete</button>test
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'ItemsList',
  
    data() {
      return {
        items: []
      };
    },
    
    created() {
      axios.get('http://127.0.0.1:8000/api/items')
        .then(response => {
          this.items = response.data;
        })
        .catch(error => {
          console.error("no data..."+error);
        });
    },
    
    methods: {
      async deleteItem(itemId) {
        try {
          await fetch(`http://127.0.0.1:8000/api/item/${itemId}`, { method: 'DELETE' });
          location.reload();
        } catch (error) {
          console.error('Error deleting item:', error);
        }
      }
    },
    
    computed: {
      filteredItems() {
        const currentDate = new Date();
        return this.items.filter(item => {
          const deadline = new Date(item.deadline);
          return deadline > currentDate && item.completed !== 1;
        });
      }
    }
  };
  </script>
  
  <style scoped>
  li {
    list-style: none;
  }
  </style>
  