<template>
  <div>
    <h1>Prueba Tecnica Desarrollador Jr.</h1>
    <h2>Registro</h2>
    <form @submit.prevent="registerUser">
      <div class="form-group">
        <label for="usuario">Nombre de usuario:</label>
        <input type="text" id="username" v-model="usuario" required>
      </div>
      <div class="form-group">
        <label for="email">Correo electrónico:</label>
        <input type="email" id="email" v-model="email" required>
      </div>
      <div class="form-group">
        <label for="password">Contraseña:</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <div class="form-group">
        <label for="confirmPassword">Confirmar contraseña:</label>
        <input type="password" id="confirmPassword" v-model="confirmPassword" required>
      </div>
      <div class="form-group">
        <button type="submit">Registrarse</button>
      </div>
    </form>
  </div>
</template>

<style>
.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  font-weight: bold;
}

input[type="text"],
input[type="email"],
input[type="password"] {
  width: 40%; 
  padding: 5px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      usuario: '',
      email: '',
      password: '',
      confirmPassword: ''
    };
  },
  methods: {
    async registerUser() {
      try{
        const response= await axios.post('https://candidates-exam.herokuapp.com/api/v1/usuarios', {
        nombre: this.usuario,
        email: this.email,
        password: this.password,
        password_confirmation: this.confirmPassword
      });
      console.log(response.data);
      if(response.data)
      {
        this.usuario="",
        this.email="",
        this.password="",
        this.confirmPassword=""
        alert("Registro Exitoso, Haga clic en Aceptar para continuar", function() {
          console.log("Aceptar");
            });
      }
      }catch(error){
        console.error(error)
      }
    },
  },
};
</script>

