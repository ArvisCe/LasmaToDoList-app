<template>
  <div id="site">
    <input type="button" value="+" @click="new_item()">
    <ItemsList :items="itemsData" />
  </div>
  <div class="new_item" v-if="new_item_box">
      <button class="close_button" @click="close_box()">X</button>
      <center>
        <input class="input_field" type="text" placeholder="nosaukums"><br>
        <textarea placeholder="apraksts"></textarea><br>
        <button type="submit">iesniegt</button>
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
    };
  },
  methods:{
    new_item(){
      this.new_item_box = true;
      document.getElementById("site").classList.add("blur");
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