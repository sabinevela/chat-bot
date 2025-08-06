<template>
  <div class="home">
    <img src="@/assets/iaimagen.png" alt="Sabibot Avatar" class="avatar" />
    <h1>Bienvenido a Sabibot</h1>
    <p>Tu asistente inteligente para atención en tiendas de comida</p>
    <div class="btn-group">
      <router-link to="/login" class="btn btn-primary">Iniciar sesión</router-link>
      <router-link to="/register" class="btn btn-secondary">Registrarse</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

.home {
  font-family: 'Inter', sans-serif;
  height: 100vh;
  background: linear-gradient(135deg, #ffe8f0 0%, #fff7fa 100%);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0 2rem;
  color: #5a2c52;
  text-align: center;
}

.avatar {
  width: 180px;
  margin-bottom: 2rem;
  border-radius: 20px;
  box-shadow: 0 6px 18px rgba(244, 143, 177, 0.35);
  transition: transform 0.3s ease;
}
.avatar:hover {
  transform: scale(1.05);
}

h1 {
  font-weight: 700;
  font-size: 3rem;
  margin-bottom: 0.5rem;
  letter-spacing: 1.2px;
  user-select: none;
}

p {
  font-weight: 500;
  font-size: 1.25rem;
  margin-bottom: 3rem;
  max-width: 400px;
  user-select: none;
  line-height: 1.5;
  color: #6d4861;
}

.btn-group {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
  max-width: 350px;
}

.btn {
  flex-grow: 1;
  padding: 1.1rem 2rem;
  border-radius: 40px;
  font-weight: 600;
  font-size: 1.15rem;
  cursor: pointer;
  text-decoration: none;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 6px 15px rgba(244, 143, 177, 0.3);
  transition: background-color 0.3s ease, box-shadow 0.3s ease;
  user-select: none;
}

.btn-primary {
  background-color: #f57ca2;
  color: white;
  border: none;
}
.btn-primary:hover {
  background-color: #e8567f;
  box-shadow: 0 10px 25px rgba(232, 86, 127, 0.5);
}

.btn-secondary {
  background-color: transparent;
  border: 2.5px solid #f57ca2;
  color: #f57ca2;
  font-weight: 700;
}
.btn-secondary:hover {
  background-color: #f57ca2;
  color: white;
  box-shadow: 0 10px 25px rgba(245, 124, 162, 0.5);
}
</style>
