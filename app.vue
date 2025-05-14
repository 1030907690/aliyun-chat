<template>
  <div>
    <NuxtRouteAnnouncer/>
    112
    <NuxtWelcome/>
  </div>
</template>
<script lang="ts" setup>
import {ref, onMounted} from 'vue'
 let data = ref<string>("")
onMounted(() => {
  console.log("mount")
  $fetch(`https://nuxt.com/docs/migration/component-options`).then(res => {
    console.log("res ",res)
    data.value = res as string
  })
  let query = {"prompt":"你好啊"}
  $fetch(`http://127.0.0.1:20710/api/front/chatbot/chat`,{
    method: 'POST',
    body: JSON.stringify(query)
  }).then(res => {
    console.log("res ",res)
  })
})

</script>
