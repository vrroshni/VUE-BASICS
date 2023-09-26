<script setup>
  import {ref} from 'vue';
  const count=ref(0)  
</script>


<template>
  <main> 
    <div>
      <h2 >The current count is....</h2>
      <h4>{{ count }}</h4>
      <button @click="count--">-</button>
      <button @click="count++">+</button>
    </div>
  </main>
</template>

<style scoped>


main {
  background-color: aliceblue;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

div {
  text-align: center;
}
</style>