<template>
  <div id="site">
    <input type="button" value="+" @click="new_item()">
    <ItemsList :items="itemsData" />
  </div>
  <div class="new_item" v-if="new_item_box">
      <button class="close_button" @click="close_box()">X</button>
      <center style="margin-top: 10%;">
        <input type="text" id="name" class="input_field"  placeholder="nosaukums"><br>
        <textarea placeholder="apraksts" id="description"></textarea><br>
        <span>Gala datums</span><br>
        <input type="datetime-local" id="deadline" class="input_field">
        <button type="submit" @click="new_item_submit()">iesniegt</button>
      </center>
  </div>
</template>

<script>
import ItemsList from './components/Items.vue';

export default {
  components: {
    ItemsList,
  },
  data() {
    return {
      itemsData: [],
      new_item_box : false,
      itemName : "",
      itemDescription : "",
      itemDeadline: "",
    };
  },
  methods:{
    new_item(){
      this.new_item_box = true;
      document.getElementById("site").classList.add("blur");
    },
    async new_item_submit(){
      this.itemName = document.getElementById("name").value;
      this.itemDescription = document.getElementById("description").value;
      this.itemDeadline = document.getElementById("deadline").value;
      try {
        const item = {
          name: this.itemName,
          deadline: this.itemDeadline,
        };
        if (this.itemDescription) {
            item.description = this.itemDescription;
        }
        const response = await fetch("http://127.0.0.1:8000/api/item/store", {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ item })
        });
        if (response.ok) {
          location.reload();
        } else {
          console.error('Error deleting item:', response.status);
        }
      } catch (error) {
        console.error('Error deleting item:', error);
      }
    },
    close_box(){
      this.new_item_box = false;
      document.getElementById("site").classList.remove("blur");
    }
  }
};
</script>

<style>

.new_item{
  position: absolute;
  background-color: #e6e1e1;
  top: 50%;
  left: 50%;
  margin-top: -25vh;
  margin-left: -25vw;
  width: 50vw;
  height: 50vh;
  padding: 5px 10px 10px 15px;
}

.close_button{
  float:right;
}

.blur{
  filter: blur(2px);
  pointer-events: none;
}

textarea{
  resize: none;
  width: 50%;
  height: 50px;
}

.input_field{
  width: 40%;
}
</style>