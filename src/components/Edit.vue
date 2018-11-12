<template>
  <div class="edit container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Editar paciente</h1>
    <form v-on:submit="updateCustomer">
        <div class="well">
            <h4>Informações do paciente</h4>
            <div class="form-group">
                <label>Nome</label>
                <input type="text" class="form-control" placeholder="Name" v-model="customer.name">
            </div>
            <div class="form-group">
                <label>Sobrenome</label>
                <input type="text" class="form-control" placeholder="Sobrenome" v-model="customer.lastname">
            </div>
        </div>
        <div class="well">
            <h4>Contato do Paciente</h4>
            <div class="form-group">
                <label>Email</label>
                <input type="text" class="form-control" placeholder="Email" v-model="customer.email">
            </div>
            <div class="form-group">
                <label>Telefone</label>
                <input type="text" class="form-control" placeholder="Telefone" v-model="customer.phone">
            </div>
        </div>

        <div class="well">
            <h4>Endereço do paciente</h4>
            <div class="form-group">
                <label>Endereço</label>
                <input type="text" class="form-control" placeholder="Endereço" v-model="customer.address">
            </div>
        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
    </form>
  </div>
</template>

<script>
    import Alert from './Alert'
    export default {
    name: 'add',
    data () {
        return {
        customer: {},
        alert:''
        }
    },
    methods: {
        fetchCustomer(id){
            
            this.$http.get('http://localhost:3000/users/'+id)
            .then(function(response){
                this.customer = response.body;
            });
        },
        updateCustomer(e){
            
            if(!this.customer.name || !this.customer.lastname || !this.customer.email){
                this.alert = 'Por favor preencha todos os campos requeridos';
            } else {
                let updCustomer = {
                    name: this.customer.name,
                    lastname: this.customer.lastname,
                    phone: this.customer.phone,
                    email: this.customer.email,
                    address: this.customer.address
                }

                this.$http.put('http://localhost:3000/users/'+this.$route.params.id, updCustomer)
                    .then(function(response){
                        this.$router.push({path: '/', query: {alert: 'Paciente atualizado'}});
                    });

                e.preventDefault();
            }
            e.preventDefault();
        }
    },
    created: function(){
            
        this.fetchCustomer(this.$route.params.id);
    },
    components:{
        Alert
    }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
