<template>
    <div>
      <ul>
        <li v-for="item in filteredItems" :key="item.id">
            <div class="card">
              <h3>{{ item.name }}</h3>
              <p v-if="description">{{ item.description }}</p>
              <p>Izpildes laiks: {{ item.deadline }}</p>
              <label>
                Izdarīts:
                <input type="checkbox" v-model="toggle" true-value="1" false-value="false" id="isCompleted" @click="updateItem(item.id)"/>
              </label><br>
              <button @click="deleteItem(item.id)">Dzēst</button>
            </div>
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
          },
          async updateItem(itemId) {
            console.log("logged");
            try {
        const item = {
          completed: document.getElementById("isCompleted").value,
        };
        
        if (this.itemDescription) {
          item.description = this.itemDescription;
        }

        const response = await fetch(`http://127.0.0.1:8000/api/item/${itemId}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ item })
        });

        if (response.ok) {
          location.reload();
        } else {
          console.error('Error adding item:', response.status);
        }
      } catch (error) {
        console.error('Error adding item:', error);
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
  .card {
    background-color: #f5f5f5;
    border: 1px solid #ddd;
    padding: 10px;
    margin-bottom: 10px;
    width: 50%;
    border-radius: 5px;
  }
  
  button {
    margin-top: 10px;
  }
  li {
    list-style: none;
  }
</style>
  
  