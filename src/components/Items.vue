<template>
    <div>
      <h2>Items</h2>
      <ul>
        <li v-for="item in items" :key="item.id">
          name: {{ item.name }}<br>
          <span v-if="item.description">Description: {{ item.description }}<br></span>
          Deadline: {{  item.deadline }}<br>
          <span v-if="item.completed == 0">Not Completed</span>
          <span v-if="item.completed == 1">Completed</span>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'ItemsList', // Rename the component to ItemsList
  
    data() {
      return {
        items: [],
      };
    },
  
    created() {
      axios.get('http://127.0.0.1:8000/api/items')
        .then(response => {
          this.items = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
  };
  </script>

<style>

li{
    list-style: none;
}

</style>
  