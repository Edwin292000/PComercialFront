<template>
  <div class="hello">
    <h1>Tercer Parcial Edwin</h1>
    <h1>datos recibidos</h1>
    <div v-for="datos in tareas" :key="datos.id">
      <div style="border-style: solid; border-width: thin; margin:25px;">
        <h1>{{datos.titulo}}</h1>
      </div>
      
    </div>
    <button @click="agregar()">Agregar</button>
    
    <ApolloMutation
      :mutation="gql => gql`
          mutation createEstado($descripcion: String!){
            createEstado(descripcion:$descripcion){
              nombre
            }
          }
        `"
      :variables="{desc}"
      @done="onDone"
      :optimisticResponse="{
          __typename: 'Mutation',
          someWork: {
            __typename: 'SomeWorkPayload',
            success: true,
            timeSpent: 100,
          },
        }"
    >
      <template v-slot="{ mutate, loading, error }">
        <button :disabled="loading" @click="mutate()">Agregar</button>
        <p v-if="error">An error occurred: {{ error }}</p>
      </template>
    </ApolloMutation> 

  </div>
</template>

<script>
import gql from 'graphql-tag'

const ADD_TODO= gql `
mutation createEstado($nombre:String!, $decripcion:String!){
  createEstado(nombre:"datas", descripcion:"datas"){
    nombre
  }
}

`
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      count: 4,
      nombre: "fin",
      desc: "termindo",
    };
  },
  methods:{
    onDone(val) {
      console.log(val)
      //this.ret = val.data.register.token;
    },
    agregar: function(){
      
      //console.log('agregar')
      const n=""
      const d=""
      this.$apollo.mutate({
        mutation: ADD_TODO,
        variables:{
          nombre:n,
          descripcion:d
        },
      }).catch((res) => {
        console.log(res)
      const errors = res.graphQLErrors.map((error) => {
        return error.message;
      });
      console.log(errors)
    });
    }
  },
  apollo:{
        tareas: gql `
          query {
              tareas{
                  id
                  titulo
                  descripcion
              }
          }
      `
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
