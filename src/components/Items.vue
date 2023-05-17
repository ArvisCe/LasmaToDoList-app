<template>
    <div>
      <ul>
        <li v-for="item in filteredItems" :key="item.id">
            <h1>
              {{ item.name }}
              <input type="checkbox" v-model="toggle" true-value="1" false-value="false" id="isCompleted" @click="updateItem(item.id)"/>
            </h1>
            <h3>{{ item.description }}</h3>
            <h6>Izpildīt līdz {{ item.deadline }}</h6>
            <h6>izveides laiks {{ item.created_at }}</h6>
            <h5>{{  item }}</h5>
            <span v-if="item.description">{{ item.description }}</span>
            <br>
            <button type="submit" class="button delete" @click="deleteItem(item.id)">Delete</button>
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
  li {
    list-style: none;
  }
  </style>
  