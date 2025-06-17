<script setup>
import axios from 'axios'
import { ref, onMounted} from 'vue'

const title = ref('プレイヤーの作成')

const name = ref('')
const number = ref('')

const submit = async () =>{
    const payload = {
        name: name.value,
        number: number.value
    }

    try {
        const result = await axios.post('http://localhost:5141/api/players', payload)
        console.log(result.data);
        alert('登録完了！')
        name.value =''
        number.value =''
    } catch (e){
        console.error('登録エラー',e)
    }
}
</script>

<template>
    <div>
        <h1>{{ title }}</h1>
        <input v-model="name" placeholder="名前" />
        <textarea v-model="number" placeholder="攻撃力"></textarea>
        <button @click="submit">送信</button>
    </div>
</template>