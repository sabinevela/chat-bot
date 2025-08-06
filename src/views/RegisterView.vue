<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const nombre = ref('');
const apellido = ref('');
const email = ref('');
const password = ref('');
const router = useRouter();

const register = () => {
  if (!nombre.value || !apellido.value || !email.value || !password.value) {
    alert('Completa todos los campos');
    return;
  }

  const users = JSON.parse(localStorage.getItem('users') || '[]');
  if (users.find(u => u.email === email.value)) {
    alert('Este correo ya está registrado');
    return;
  }

  users.push({
    nombre: nombre.value,
    apellido: apellido.value,
    email: email.value,
    password: password.value
  });

  localStorage.setItem('users', JSON.stringify(users));
  alert('Registro exitoso');
  router.push('/login');
};
</script>

<template>
  <div class="container">
    <h2>Registrarse</h2>
    <form @submit.prevent="register" autocomplete="off">
      <input v-model="nombre" type="text" placeholder="Nombre" required />
      <input v-model="apellido" type="text" placeholder="Apellido" required />
      <input v-model="email" type="email" placeholder="Correo electrónico" required />
      <input v-model="password" type="password" placeholder="Contraseña" required />
      <button type="submit">Crear cuenta</button>
    </form>
    <p>¿Ya tienes cuenta? <router-link to="/login">Iniciar sesión</router-link></p>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

.container {
  font-family: 'Inter', sans-serif;
  max-width: 420px;
  margin: 6rem auto;
  padding: 3rem 3rem;
  background: #fff9fb;
  border-radius: 20px;
  box-shadow: 0 6px 20px rgba(245, 124, 162, 0.3);
  color: #5b2c5d;
  text-align: center;
  user-select: none;
}

h2 {
  font-weight: 700;
  font-size: 2.2rem;
  margin-bottom: 2.8rem;
  letter-spacing: 0.8px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 1.6rem;
}

input[type="text"],
input[type="email"],
input[type="password"] {
  padding: 14px 18px;
  border-radius: 15px;
  border: 2px solid #f4b1cc;
  font-size: 1.1rem;
  font-weight: 500;
  color: #6d4861;
  outline-offset: 2px;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input:focus {
  border-color: #f57ca2;
  box-shadow: 0 0 8px rgba(245, 124, 162, 0.7);
}

button {
  padding: 14px 0;
  border-radius: 40px;
  font-weight: 700;
  font-size: 1.2rem;
  background-color: #f57ca2;
  color: white;
  border: none;
  cursor: pointer;
  box-shadow: 0 7px 20px rgba(245, 124, 162, 0.5);
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
}

button:hover {
  background-color: #d94e78;
  box-shadow: 0 9px 25px rgba(217, 78, 120, 0.6);
}

p {
  margin-top: 2.5rem;
  font-weight: 500;
  font-size: 1rem;
}

a {
  color: #f57ca2;
  font-weight: 700;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
