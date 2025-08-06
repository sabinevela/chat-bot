<template>
  <div class="chatbot">
    <h2>Asistente Virtual - Tienda</h2>

    <div class="chat-window" ref="chatWindow">
      <div v-for="(msg, index) in messages" :key="index" :class="msg.sender">
        <strong>{{ msg.sender === 'user' ? 'Tú' : 'Bot' }}:</strong> {{ msg.text }}
      </div>

      <div class="avatar-center">
        <img
          src="https://i.imgur.com/tuAvatarAqui.png"
          alt="Avatar Sabine"
          class="avatar"
        />
      </div>
    </div>

    <input
      v-model="input"
      @keyup.enter="sendMessage"
      placeholder="Escribe tu mensaje..."
      class="chat-input"
    />
    <button @click="sendMessage" class="send-btn">Enviar</button>

    <div class="upload-section">
      <h3>Subir imagen para clasificación</h3>
      <input type="file" accept="image/*" @change="handleImageUpload" />
      <div v-if="imagePreview">
        <p>Imagen cargada:</p>
        <img :src="imagePreview" alt="Preview" width="150" class="preview-img" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input: '',
      messages: [{ sender: 'bot', text: '¡Hola! ¿En qué puedo ayudarte hoy?' }],
      imagePreview: null,
    };
  },
  methods: {
    sendMessage() {
      if (!this.input.trim()) return;

      this.messages.push({ sender: 'user', text: this.input });

      setTimeout(() => {
        this.messages.push({ sender: 'bot', text: 'Gracias por tu mensaje. Estoy procesando tu solicitud...' });
        this.scrollToBottom();
      }, 500);

      this.input = '';
      this.$nextTick(() => {
        this.scrollToBottom();
      });
    },
    handleImageUpload(event) {
      const file = event.target.files[0];
      if (file) {
        this.imagePreview = URL.createObjectURL(file);
        this.messages.push({ sender: 'bot', text: 'Imagen recibida. Procesando clasificación...' });
        this.$nextTick(() => {
          this.scrollToBottom();
        });
      }
    },
    scrollToBottom() {
      const chat = this.$refs.chatWindow;
      if (chat) {
        chat.scrollTop = chat.scrollHeight;
      }
    },
  },
};
</script>

<style scoped>
.chatbot {
  max-width: 600px;
  margin: 40px auto;
  padding: 25px 30px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  border-radius: 15px;
  background: #fff0f6; /* rosa pastel claro */
  box-shadow: 0 8px 20px rgba(255, 192, 203, 0.3);
  color: #5a2a45;
}

h2 {
  text-align: center;
  font-weight: 700;
  color: #d6336c; /* rosa más fuerte para título */
  margin-bottom: 20px;
}

.chat-window {
  height: 350px;
  overflow-y: auto;
  background: #ffe6f0; /* rosa pastel muy claro */
  border-radius: 15px;
  padding: 15px 20px;
  box-shadow: inset 0 0 15px rgba(214, 51, 108, 0.1);
  position: relative;
  margin-bottom: 20px;
}

.bot,
.user {
  max-width: 70%;
  padding: 10px 18px;
  margin: 8px 0;
  border-radius: 25px;
  font-size: 16px;
  line-height: 1.4;
  word-wrap: break-word;
  box-shadow: 0 4px 6px rgba(214, 51, 108, 0.1);
}

.bot {
  background: #f9d6e8;
  color: #6b2063;
  border-bottom-left-radius: 5px;
  text-align: left;
}

.user {
  background: #fce4ec;
  color: #9f457e;
  margin-left: auto;
  border-bottom-right-radius: 5px;
  text-align: right;
}

.avatar-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none; /* que no interfiera con scroll ni clics */
  z-index: 10;
}

.avatar {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  border: 4px solid #d6336c;
  box-shadow: 0 4px 15px rgba(214, 51, 108, 0.4);
  object-fit: cover;
  user-select: none;
}

.chat-input {
  width: calc(100% - 90px);
  padding: 12px 15px;
  font-size: 16px;
  border: 2px solid #d6336c;
  border-radius: 30px;
  outline: none;
  transition: border-color 0.3s ease;
  color: #5a2a45;
}

.chat-input:focus {
  border-color: #a61e4d;
}

.send-btn {
  width: 70px;
  margin-left: 10px;
  background-color: #d6336c;
  border: none;
  border-radius: 30px;
  color: white;
  font-weight: 700;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.send-btn:hover {
  background-color: #a61e4d;
}

.upload-section {
  margin-top: 25px;
  background: #ffe6f0;
  padding: 15px 20px;
  border-radius: 15px;
  border: 1px solid #d6336c;
  box-shadow: 0 4px 12px rgba(214, 51, 108, 0.1);
  color: #5a2a45;
}

.upload-section h3 {
  margin-bottom: 12px;
  font-weight: 700;
}

.upload-section input[type='file'] {
  cursor: pointer;
  border-radius: 10px;
  padding: 5px;
  border: 1px solid #d6336c;
  background: white;
  transition: border-color 0.3s ease;
}

.upload-section input[type='file']:hover {
  border-color: #a61e4d;
}

.preview-img {
  margin-top: 15px;
  border-radius: 15px;
  border: 2px solid #d6336c;
  box-shadow: 0 4px 12px rgba(214, 51, 108, 0.25);
  object-fit: cover;
}
</style>

