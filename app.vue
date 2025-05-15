<template>
  <div>
    <NuxtLayout>
      <NuxtPage/>
    </NuxtLayout>
  </div>
</template>

<script lang="ts" setup>
import {ref, onMounted} from 'vue'

let data = ref<string>("")




useHead({
 // script: [
 //   {src: "https://g.alicdn.com/aliyun-documentation/web-chatbot-ui/0.0.24/index.js",type:"text/javascript",defer:true,async:true},
 // ],
 link:[
   { rel: 'stylesheet', href: 'https://g.alicdn.com/aliyun-documentation/web-chatbot-ui/0.0.24/index.css' }
 ]
})
onMounted(() => {
  console.log("mount")
  $fetch(`https://nuxt.com/docs/migration/component-options`).then(res => {
    // console.log("res ",res)
    // data.value = res as string
  })
  // let query = {"prompt":"你好啊"}
  // $fetch(`http://127.0.0.1:20710/api/front/chatbot/chat`,{
  //   method: 'POST',
  //   body: JSON.stringify(query)
  // }).then(res => {
  //   console.log("res ",res)
  // })

  /**
   * 在onMounted阶段添加js ， 用useHead方式  g.alicdn.com/aliyun-documentation/web-chatbot-ui/0.0.24/index.js内部使用了  document.body 报错为null
   */
  const script = document.createElement("script");
  script.type = "text/javascript";
  script.src = "https://g.alicdn.com/aliyun-documentation/web-chatbot-ui/0.0.24/index.js";
  document.body.appendChild(script);
  initChat()
})

const initChat = () => {

  window.CHATBOT_CONFIG = {
    endpoint: "http://127.0.0.1:20710/api/front/chatbot/chat", // 可以替换为 https://{your-fc-http-trigger-domain}/chat
    displayByDefault: false, // 默认不展示 AI 助手聊天框
    aiChatOptions: { // aiChatOptions 中 options 会传递 aiChat 组件，自定义取值参考：https://docs.nlkit.com/nlux/reference/ui/ai-chat
      conversationOptions: { // 自定义取值参考：https://docs.nlkit.com/nlux/reference/ui/ai-chat#conversation-options
        conversationStarters: [
          {prompt: '哪款手机续航最长？'},
          {prompt: '你们有哪些手机型号？'},
          {prompt: '有折叠屏手机吗?'},
        ]
      },
      displayOptions: { // 自定义取值参考：https://docs.nlkit.com/nlux/reference/ui/ai-chat#display-options
        height: 600,
      },
      personaOptions: { // 自定义取值参考：https://docs.nlkit.com/nlux/reference/ui/ai-chat#chat-personas
        assistant: {
          name: '你好，我是你的 AI 助手',
          // AI 助手的图标
          avatar: 'https://img.alicdn.com/imgextra/i2/O1CN01Pda9nq1YDV0mnZ31H_!!6000000003025-54-tps-120-120.apng',
          tagline: '您可以尝试点击下方的快捷入口开启体验！',
        }
      }
    },
    dataProcessor: {
      /**
       * 在向后端大模型应用发起请求前改写 Prompt。
       * 比如可以用于总结网页场景，在发送前将网页内容包含在内，同时避免在前端显示这些内容。
       * @param {string} prompt - 用户输入的 Prompt
       * @param {string}  - 改写后的 Prompt
       */
      rewritePrompt(prompt) {
        return prompt;
      }
    }
  };

}

</script>

<style>
:root {
  /* webchat 工具栏的颜色 */
  --webchat-toolbar-background-color: #1464E4;
  /* webchat 工具栏文字和按钮的颜色 */
  --webchat-toolbar-text-color: #FFF;
}

/* webchat 对话框如果被遮挡，可以尝试通过 z-index、bottom、right 等设置来调整位置 */
.webchat-container {
  z-index: 100;
  bottom: 10px;
  right: 10px;
}

/* webchat 的唤起按钮如果被遮挡，可以尝试通过 z-index、bottom、right 等设置来调整位置 */
.webchat-bubble-tip {
  z-index: 99;
  bottom: 20px;
  right: 20px;
}
</style>
<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}

.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(1rem);
}
</style>
