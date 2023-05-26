<template>
 <div class="perfil">
      <div>
        <img alt="Vue logo" src="../assets/iconbl.png">
        <h2>Bienevenido a tu perfil  {{ nombre }}   </h2>
          <h2>ACA ESTAN TUS DATOS</h2>
          <h3>{{ email }}</h3>
          <input type="file" @change="handleFileChange" accept=".pdf" required>&nbsp;
          <br><button @click="uploadFile">Enviar</button> &nbsp;
          <button @click="seeFile">Ver CV</button>
      </div>
    </div>
  </template>

<style>
.perfil {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f2f2f2;
}

.perfil div {
  text-align: center;
  padding: 20px;
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
}

.perfil h2 {
  font-size: 24px;
  margin-bottom: 20px;
}

.perfil input[type="file"] {
  margin-bottom: 10px;
}

.perfil button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.perfil button:hover {
  background-color: #45a049;
}
</style>
  
<script>
  import axios from 'axios';
  
  export default {
    data() {
    return {
      nombre: "----",
      email:"-",
      cvFile: null,
      url: ""
    };
  },
    mounted() {
    this.traerDatos();
  },
    methods: {
      async traerDatos() {
        this.usuarioobtenido = localStorage.getItem('Sesion');
        console.log(this.usuarioobtenido);
        const headers = { 
          'Content-Type': 'application/json', 
          'Authorization': this.usuarioobtenido }; // Realizar la solicitud utilizando axios
          axios.get('https://candidates-exam.herokuapp.com/api/v1/usuarios/', { headers }) 
          .then(response => { // Manejar la respuesta de la petición 
            this.nombre=response.data.nombre;
            console.log(this.nombre);
            this.email=response.data.email;
            this.url=response.data.url;
            console.log(response); }) 
          .catch(error => { // Manejar el error de la petición 
          console.error(error); });
      },

      handleFileChange(event) {
      this.cvFile = event.target.files[0];
    },
   /* uploadFile() {
      this.usuarioobtenido = localStorage.getItem('Sesion');
      console.log(this.usuarioobtenido);
      let formData = new FormData();
      formData.append('file', this.file);

      axios.post('https://candidates-exam.herokuapp.com/api/v1/usuarios/'+this.url+'/cargar_cv', formData,{
        headers:{
        Authorization: `Bearer ${this.usuarioobtenido}`,
        }
      })
        .then(response => {
          alert("Archivo enviado correctamente, Haga clic en Aceptar para continuar", function() {
          console.log("Aceptar");
            });
          
        })
        .catch(error => {
          console.error('Error al enviar el archivo', error);
        });
     },*/
     async uploadFile() {
      this.usuarioobtenido = localStorage.getItem('Sesion');
      console.log(this.usuarioobtenido);
      try {
        const formData = new FormData();
        formData.append('curriculum', this.cvFile);

        const response = await axios.post(
          'https://candidates-exam.herokuapp.com/api/v1/usuarios/' + this.url + '/cargar_cv',
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data',
              Authorization: `Bearer ${this.usuarioobtenido}`,
            },
          }
        );
        this.url = response.data.url;
        console.log(response.data);
      } catch (error) {
        console.error(error);
  }
},

seeFile() {
  this.usuarioobtenido = localStorage.getItem('Sesion');
  console.log(this.usuarioobtenido);

  axios
    .get('https://candidates-exam.herokuapp.com/api/v1/usuarios/mostrar_cv', {
      headers: {
        Authorization: this.usuarioobtenido,
      },
      responseType: 'blob', // El tipo de repsuesta es blob
    })
    .then(response => {
      const url = window.URL.createObjectURL(new Blob([response.data]));

      // Se creo un tipo enlace temporal para la descarga
      const link = document.createElement('a');
      link.href = url;
      link.setAttribute('download', 'curriculum.pdf');
      document.body.appendChild(link);
      link.click();
      
      window.URL.revokeObjectURL(url);// sirve para dejar el objeto URL después de la descarga
    })
    .catch(error => {
      console.error(error);
    });
   },
  },
};
</script>