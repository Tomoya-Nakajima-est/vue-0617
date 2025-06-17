<script setup>
import axios from "axios";
import {ref, onMounted } from "vue";

const title = ref('乱数対決')
const players = ref([]);
const result = ref('');
const selectedPlayer = ref('');

onMounted(async () => {
  try {
    const res = await axios.get('http://localhost:5141/api/players')
    console.log(res.data)
    players.value = res.data
    const firstPlayable = players.value.find( p => p.name !== 'Computer');
    if(firstPlayable){
      selectedPlayer.value = firstPlayable.name;
    }
  }catch (error){
    console.error("データ取得エラー",error)
  }
})
const fight = () => {

  const playerNum = Math.floor(Math.random() * 100) + 1
  const computerNum = Math.floor(Math.random() * 100 ) + 1

  const playerName = selectedPlayer.value
  const computerName = players.value.find( p => p.name === 'Computer')?.name ?? 'Computer'

  let outcome = ''
  if(playerNum > computerNum){
    outcome = `${playerName} の勝ち！`
  }else if (playerNum < computerNum){
    outcome =`${computerName} の勝ち！`
  }else {
    outcome = '引き分け！'
  }

  result.value = `${playerName}: ${playerNum}\nvs\n${computerName}: ${computerNum}\n→ ${outcome}`
  console.log(result.value);
}
</script>

<template>
  <div>
    <h1>{{ title }}</h1>

    <label>プレイヤー選択:</label>
    <select v-model="selectedPlayer">
    <option v-for="p in players.filter(p => p && p.name && p.name !== 'Computer')"
    :key="p.id"
    :value="p.name">
    {{ p.name }}
  </option>
</select>

    <button @click="fight">対決！</button>
    <pre v-if="result">{{ result }}</pre>
  </div>
</template>
