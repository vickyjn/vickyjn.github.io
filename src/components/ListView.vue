<template>
    <div>
      <h2>List View</h2>
      <ul>
        <!-- <li v-for="item in items" :key="item.id">{{ item.name }}</li> -->
           <li v-for="(item, index) in items" :key="index" :class="{ 'list-item': true, 'active-tab': activeIndex === index }" @click="activeIndex = index">
        {{ item.name}}
      </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { ref,onMounted} from 'vue';
  import axios from 'axios';
  
  export default {
    name: 'ListView',
    setup() {
      const items = ref([]);
  
      const renderList = async () => {
        const response = await axios.get('list.json');
        items.value = response.data;
      };
  
      onMounted(() => {
        renderList();
      });
  
      return { items };
    }
  };
  </script>
  
  <style scoped>
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  
  li {
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  /* style for mouse over */
.list-item:hover {
  background-color: hsla(160, 100%, 37%, 0.1);
}

/* style for active tab */
.active-tab {
  background-color: hsla(160, 100%, 37%, 1);
  color: white;
}

  </style>
  