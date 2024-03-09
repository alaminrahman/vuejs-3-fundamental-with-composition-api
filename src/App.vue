<script setup>
  import { computed, ref } from 'vue'

  const header = ref('Shopping List App');
  const characterCount = computed(() => {
    return newItem.value.length;
  })

  const reversedItems = computed(() => {
    return [...items.value].reverse();
  })

  const editing = ref(false)
  const newItem = ref("");
  const newItemHighPriority = ref(false);
  const items = ref([
    {
      id: 1, 
      label: "10 party hats", 
      purchased: true, 
      highPriority: false
    },
    {
      id: 2, 
      label: "10 burgers", 
      purchased: true, 
      highPriority: false},
    {
      id: 3, 
      label: "10 cups", 
      purchased: false, 
      highPriority: true
    },
  ]);
  const saveItem = () => {
    if(newItem.value == ""){
      return;
    }
    items.value.push({ id: items.value.length + 1, label: newItem.value, highPriority: newItemHighPriority.value})
    newItem.value = ""
    newItemHighPriority.value = false
  }
  const doEdit = (e) => {
    editing.value = e;
    newItem.value = ""
  }
  const togglePurchased = (item) => {
    item.purchased = !item.purchased;
  }

</script>

<template>

  <div class="">
    <div class="header">
      <h1>{{ header }}</h1>
      <button
        v-if="editing"
        @click="doEdit(false)"
        class="btn">
        Cancel
      </button>

      <button 
        @click="doEdit(true)"
        v-else 
        class="btn btn-primary">
        Add Item
      </button>
    </div>

    <a v-bind:href="newItem">Dynamic Link</a>
   

    <form 
      class="add-item-form"
      v-if="editing"
      @submit.prevent="saveItem"
    >
      <input 
        v-model.trim="newItem" 
        type="text"
        placeholder="Add an item" >
      
      Priority:
      <label>
        <input type="checkbox" v-model="newItemHighPriority" >
        High Priority
      </label>
      
      <button 
        :disabled="newItem.length < 5"
        class="btn btn-primary">
        
        Save Item
      </button>

    </form>

    <p class="counter">
        {{ characterCount }} / 200
    </p>

    <ul>

      <li 
        @click="togglePurchased(item)"
        v-for="(item, index) in reversedItems" 
        v-bind:key="item.id"
        :class="{
          strikeout : item.purchased, 
          priority: item.highPriority,
          'static-class': true
          }"
        >
        {{ item.label }}
      </li>

      <p v-if="!items.length">
        Nothing to see here 
      </p>
    </ul>

  </div>


</template>

<style scoped>
</style>
