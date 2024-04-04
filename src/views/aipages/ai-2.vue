<template>
    <div>
      <!-- 聊天记录列表 -->
      <div v-for="(message, index) in chatHistory" :key="index">
        <p>{{ message.user }}: {{ message.content }}</p>
      </div>

      <input v-model="currentMessage" type="text" placeholder="输入你的问题...">
      <button @click="sendMessage">发送</button>
    </div>
  </template>

  <script>
  import axios from 'axios';

  export default {
    data() {
      return {
        currentMessage: '',
        chatHistory: [], // 聊天记录数组
      };
    },
    methods: {
      async sendMessage() {
        try {
          const response = await axios.post('YOUR_CHATGPT_API_URL', {
            message: this.currentMessage
          });
          const chatResponse = response.data; // 假设API返回整个对话响应

          // 更新聊天记录数组
          this.chatHistory.push({
            user: '你',
            content: this.currentMessage
          });
          this.chatHistory.push({
            user: '大模型',
            content: chatResponse.answer // 假设API返回的数据结构中有`answer`字段
          });

          // 清空输入框
          this.currentMessage = '';

          // 滚动到聊天记录底部
          const chatContainer = this.$el.querySelector('.chat-container');
          chatContainer.scrollTop = chatContainer.scrollHeight;
        } catch (error) {
          console.error(error);
        }
      }
    }
  };
  </script>

  <style scoped>
  .chat-container {
    height: 300px; /* 设置一个合适的高度来显示聊天记录 */
    overflow-y: scroll; /* 启用滚动条 */
  }
  </style>
