<template>
  <div>
    <ItemsList :items="itemsData" />
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
    };
  },
};
</script>
