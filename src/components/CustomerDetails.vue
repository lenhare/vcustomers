<template>
  <div class="details container">
    <router-link to="/">Voltar</router-link>
    <h1 class="page-header">{{customer.name}} {{customer.lastname}}
        <span class="pull-right">
            <router-link class="btn btn-primary" v-bind:to="'/edit/'+customer.id">Editar</router-link>
            <button class="btn btn-danger" v-on:click="deleteCustomer(customer.id)">Deletar</button>
            </span>
    </h1>
    <ul class="list-group">
            <li class="list-group-item"><span class="glyphicon glyphicon-phone" aria-hidden="true"></span> {{customer.phone}}</li>
            <li class="list-group-item"><span class="glyphicon glyphicon-envelope" aria-hidden="true"></span> {{customer.email}}</li>
        </ul>

        <ul class="list-group">
            <li class="list-group-item"> {{customer.address}} </li>
        </ul>
  </div>
</template>

<script>
export default {
  name: 'customerdetails',
  data () {
    return {
      customer: ''
    }
  },
  methods:{
      fetchCustomer(id){
          this.$http.get('http://localhost:3000/users/'+id)
          .then(function(response){
            this.customer = response.body;
          });
      },
      deleteCustomer(id){
          this.$http.delete('http://localhost:3000/users/'+id)
          .then(function(response){
            this.$router.push({path: '/', query: {alert: 'Paciente excluído'}});
          });
      }
  },
  created: function(){

      this.fetchCustomer(this.$route.params.id);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
