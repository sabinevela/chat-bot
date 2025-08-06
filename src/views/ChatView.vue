<script setup>
import { ref, nextTick, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import iaAvatar from '@/assets/iaimagen.png';


const router = useRouter();
const messages = ref([]);
const newMessage = ref('');
const chatBox = ref(null);

onMounted(() => {
  const user = JSON.parse(localStorage.getItem('currentUser'));
  if (!user) {
    router.push('/login');
  } else {
    const savedMessages = JSON.parse(localStorage.getItem(`chat_${user.email}`) || '[]');
    messages.value = savedMessages;
  }
  nextTick(() => scrollToBottom());
});

const saveMessages = () => {
  const user = JSON.parse(localStorage.getItem('currentUser'));
  if (user) {
    localStorage.setItem(`chat_${user.email}`, JSON.stringify(messages.value));
  }
};

const scrollToBottom = () => {
  if (chatBox.value) {
    chatBox.value.scrollTop = chatBox.value.scrollHeight;
  }
};

const sendMessage = () => {
  if (!newMessage.value.trim()) return;
  const msg = {
    id: Date.now(),
    type: 'text',
    content: newMessage.value.trim(),
    sender: 'user',
    timestamp: new Date().toISOString(),
  };
  messages.value.push(msg);
  saveMessages();
  newMessage.value = '';
  nextTick(() => scrollToBottom());

  setTimeout(() => {
    messages.value.push({
      id: Date.now() + 1,
      type: 'text',
      content: `Hola, recibí tu mensaje: "${msg.content}"`,
      sender: 'bot',
      timestamp: new Date().toISOString(),
    });
    saveMessages();
    nextTick(() => scrollToBottom());
  }, 1200);
};

const onImageChange = (e) => {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = () => {
    const msg = {
      id: Date.now(),
      type: 'image',
      content: reader.result,
      sender: 'user',
      timestamp: new Date().toISOString(),
    };
    messages.value.push(msg);
    saveMessages();
    nextTick(() => scrollToBottom());

    setTimeout(() => {
      messages.value.push({
        id: Date.now() + 1,
        type: 'text',
        content: 'He recibido la imagen, gracias.',
        sender: 'bot',
        timestamp: new Date().toISOString(),
      });
      saveMessages();
      nextTick(() => scrollToBottom());
    }, 1200);
  };
  reader.readAsDataURL(file);
  e.target.value = '';
};

const logout = () => {
  localStorage.removeItem('currentUser');
  router.push('/');
};
</script>

<template>
  <div class="chat-container">
    <header class="chat-header">
      <img :src="iaAvatar" alt="Sabibot IA" class="ia-avatar" />
      <h1>Sabibot</h1>
      <button class="logout-btn" @click="logout" title="Cerrar sesión">Cerrar sesión</button>
    </header>

    <main class="chat-box" ref="chatBox">
      <div
        v-for="msg in messages"
        :key="msg.id"
        :class="['message', msg.sender]"
      >
        <template v-if="msg.type === 'text'">
          <p>{{ msg.content }}</p>
        </template>
        <template v-else-if="msg.type === 'image'">
          <img :src="msg.content" alt="Imagen enviada" class="chat-image" />
        </template>
      </div>
    </main>

    <footer class="chat-footer">
      <input
        v-model="newMessage"
        type="text"
        placeholder="Escribe tu mensaje..."
        @keyup.enter="sendMessage"
        class="chat-input"
      />
      <label for="imageUpload" class="upload-label" title="Enviar imagen">📷</label>
      <input
        type="file"
        id="imageUpload"
        accept="image/*"
        @change="onImageChange"
        class="upload-input"
      />
      <button @click="sendMessage" class="send-btn">Enviar</button>
    </footer>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

.chat-container {
  max-width: 650px;
  margin: 40px auto;
  background: #fff9fb;
  border-radius: 25px;
  box-shadow: 0 10px 28px rgba(245, 124, 162, 0.25);
  display: flex;
  flex-direction: column;
  height: 85vh;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  color: #5b2c5d;
}

.chat-header {
  background-color: #f8bbd0;
  padding: 18px 30px;
  border-radius: 25px 25px 0 0;
  display: flex;
  align-items: center;
  gap: 18px;
  box-shadow: 0 5px 15px rgba(244, 143, 177, 0.4);
  position: relative;
}

.ia-avatar {
  width: 56px;
  height: 56px;
  border-radius: 18px;
  object-fit: cover;
  border: 3px solid #f57ca2;
  box-shadow: 0 0 10px #f57ca2;
  user-select: none;
}

.chat-header h1 {
  flex-grow: 1;
  font-weight: 700;
  font-size: 2rem;
  margin: 0;
  user-select: none;
}

.logout-btn {
  background: #d94e78;
  color: white;
  border: none;
  border-radius: 25px;
  padding: 10px 28px;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 6px 20px rgba(217, 78, 120, 0.5);
  transition: background-color 0.3s ease;
  user-select: none;
}

.logout-btn:hover {
  background: #b2345b;
}

.chat-box {
  flex-grow: 1;
  padding: 25px 30px;
  overflow-y: auto;
  background: #fff0f6;
  border-radius: 0 0 25px 25px;
  scroll-behavior: smooth;
  display: flex;
  flex-direction: column;
  gap: 18px;
  user-select: text;
}

.message {
  max-width: 70%;
  padding: 16px 24px;
  border-radius: 28px;
  font-size: 1rem;
  line-height: 1.5;
  word-wrap: break-word;
  box-shadow: 0 4px 12px rgba(245, 124, 162, 0.25);
}

.message.user {
  background-color: #f57ca2;
  align-self: flex-end;
  color: #4a1c40;
  border-bottom-right-radius: 6px;
  position: relative;
  font-weight: 600;
}

.message.bot {
  background-color: #fce4ec;
  align-self: flex-start;
  color: #721b4a;
  border-bottom-left-radius: 6px;
  position: relative;
  font-weight: 500;
}

.chat-image {
  max-width: 220px;
  max-height: 220px;
  border-radius: 18px;
  box-shadow: 0 3px 15px rgba(245, 124, 162, 0.35);
  object-fit: contain;
  user-select: none;
}

.chat-footer {
  display: flex;
  align-items: center;
  padding: 18px 30px;
  background-color: #fff9fb;
  border-radius: 0 0 25px 25px;
  gap: 12px;
  box-shadow: 0 -4px 18px rgba(245, 124, 162, 0.15);
}

.chat-input {
  flex-grow: 1;
  padding: 14px 20px;
  border-radius: 30px;
  border: 2px solid #f4b1cc;
  font-size: 1.1rem;
  color: #5b2c5d;
  outline-offset: 3px;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.chat-input:focus {
  border-color: #f57ca2;
  box-shadow: 0 0 10px rgba(245, 124, 162, 0.7);
}

.upload-label {
  cursor: pointer;
  font-size: 1.6rem;
  color: #f57ca2;
  user-select: none;
  transition: color 0.3s ease;
}

.upload-label:hover {
  color: #d94e78;
}

.upload-input {
  display: none;
}

.send-btn {
  background-color: #f57ca2;
  border: none;
  color: white;
  font-weight: 700;
  font-size: 1rem;
  padding: 12px 25px;
  border-radius: 30px;
  cursor: pointer;
  box-shadow: 0 7px 20px rgba(245, 124, 162, 0.5);
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  user-select: none;
}

.send-btn:hover {
  background-color: #d94e78;
  box-shadow: 0 9px 25px rgba(217, 78, 120, 0.6);
}
</style>
