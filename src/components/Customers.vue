<template>
  <div class="customers container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Gerenciar pacientes</h1>
    <input class="form-control" placeholder="Digite o nome do paciente" v-model="filterInput">
    <br />
    <table class="table table-striped">
        <thead>
          <tr>
            <th>Name</th>
            <th>Sobrenome</th>
            <th>Email</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="customer in filterBy(customers, filterInput)">
            <td>{{customer.name}}</td>
            <td>{{customer.lastname}}</td>
            <td>{{customer.email}}</td>
            <td><router-link class="btn btn-default" v-bind:to="'/customer/'+customer.id">Visualizar</router-link></td>
          </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert';
  export default {
    name: 'customers',
    data () {
      return {
        customers: [],
        alert:'',
        filterInput:''
      }
    },
    methods: {
      fetchCustomers(){
        this.$http.get('http://localhost:3000/users/')
          .then(function(response){
            this.customers = response.body;
          });
      },
      filterBy(list, value){
        value = value.charAt(0).toUpperCase() + value.slice(1);
        return list.filter(function(customer){
          return customer.name.indexOf(value) > -1;
        });
      }
    },
    created: function(){
      if(this.$route.query.alert){
        this.alert = this.$route.query.alert;
      }
      this.fetchCustomers();
    },
    updated: function(){
      this.fetchCustomers();
    },
    components: {
      Alert
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
