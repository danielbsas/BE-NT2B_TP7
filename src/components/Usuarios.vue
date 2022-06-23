<template>

  <section class="src-components-usuarios">
    <div class="jumbotron">
      <h2>Lista Usuarios en Mockapi.IO</h2>
      <hr>
      <hr>
      <br>
      <div v-if="usuarios.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>id</th>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Email</th>
          </tr>
          <tr v-for="(usuario, index) in usuarios" :key="index">
              <td>{{ usuario.id }}</td>
              <td>{{ usuario.Nombre }}</td>
              <td>{{ usuario.Edad }}</td>
              <td>{{ usuario.Email }}</td>
              <td><button class="btn btn-danger" @click="deleteUsuarioAxios(usuario.id)">BORRAR</button></td>
          </tr>
        </table>
        <h5 class="alert alert-primary">Se encontraron {{usuarios.length}} usuarios.</h5>
      </div>
      <h4 v-else class="alert alert-danger text-center">No se encontraron usuarios</h4>

     </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-usuarios',
    props: [],
    mounted () {
      this.getUsuariosAxios()
    },
    data () {
      return {
        url: 'https://6284336ba48bd3c40b6c2bf9.mockapi.io/UsuariosTP7/',
        usuarios: []
      }
    },
    methods: {
      getUsuariosAxios() {
        this.axios(this.url)
          .then(respuesta => {
          this.usuarios = respuesta.data
          })
        .catch(error => console.error('Error Axios', error)) 
      },
      async deleteUsuarioAxios(id) {
        try {
          let { data : usuario } = await this.axios.delete(this.url + id);
          let index = this.usuarios.findIndex(user => user.id == usuario.id)
          if(index == -1) throw new Error('Usuario no encontrado')
          this.usuarios.splice(index, 1)
        }
        catch(error) {
          console.error('Error al borrar usuario (Axios)', error)
        }   
      }
    },
    computed: {
    }
}

</script>

<style scoped lang="css">

</style>
