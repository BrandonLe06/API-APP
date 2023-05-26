<template>
  <div class="about">
    <div>
      <img alt="Vue logo" src="../assets/iconbl.png">
      <h2>Iniciar Sesión</h2>
      <form @submit.prevent="iniciarSesion">
        <input type="email" v-model="email" placeholder="Email">
        <input type="password" v-model="password" placeholder="Contraseña">
        <button type="submit">Iniciar Sesión</button>
      </form>
    </div>
  </div>
</template>

<style>
.about {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f2f2f2;
}

.about div {
  text-align: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
}

.about h2 {
  font-size: 24px;
  margin-bottom: 20px;
}

.about input[type="email"],
.about input[type="password"] {
  width: 80%;
  padding: 10px;
  font-size: 16px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.about button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.about button:hover {
  background-color: #45a049;
}

</style>


<script>
  import axios from 'axios';
  import { useRouter } from 'vue-router';

export default {
  // ...

  methods: {

    async iniciarSesion() {
      try {
        const response = await axios.post('https://candidates-exam.herokuapp.com/api/v1/auth/login', {
          email: this.email,
          password: this.password,
        });
          // validaciones necesarias con la respuesta del servidor
          if (response.data.tipo) {
          const token = response.data.token;
          localStorage.setItem('Sesion',token); // Llama a la acción guardarToken para almacenar el token
          const router = useRouter();
          this.usuarioobtenido = localStorage.getItem('Sesion');
          this.$router.push('/perfil'); // Redirecciona al formulario deseado
        } else {
          // caso en el que el inicio de sesión no sea exitoso
          console.log('Inicio de sesión incorrecto');
        }
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
  