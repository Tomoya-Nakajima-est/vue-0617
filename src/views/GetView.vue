<script setup>
import axios from "axios";
import {ref, onMounted } from "vue";

const title= ref("乱数対決");
const db = ref([]);

onMounted(async () => {
  try {
    const res = await axios.get('http://localhost:5141/api/players')
    console.log(res.data)
    db.value = res.data
  }catch (error){
    console.error("データ取得エラー",error)
  }
})
</script>

<template>
  <div>
    <h1>{{ title }}</h1>
    <ul>
      <li v-for="dbItem in db" :key="dbItem.id">
        {{ dbItem.name }} {{ dbItem.number }}
      </li>
    </ul>
  </div>
</template>
