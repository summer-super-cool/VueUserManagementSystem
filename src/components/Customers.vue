<template>
  <div class="customers container">
    <Alert v-if="alert" v-bind:message="alert"></Alert>
    <h1 class="page-header">User management system</h1>

    <input type="text" class="form-control" placeholder="搜索" v-model="filterInput">
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>Tel</th>
          <th class="hidden-xs">mailbox</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="customer in filterBy(customers,filterInput)">
          <td>{{customer.name}}</td>
          <td>{{customer.phone}}</td>
          <td class="hidden-xs">{{customer.email}}</td>
          <td><router-link class="btn btn-default" v-bind:to="'/customer/'+customer.id">details</router-link></td>
        </tr>
      </tbody>    


    </table>
    
  </div>
</template>

<script>
import Alert from './Alert'

export default {
  name: 'customers',
  data () {
    return {
        customers:[],
        alert:"",
        filterInput:"",
    }
  },
  methods:{
    fetchCustomers(){
      this.$http.get("http://localhost:3000/users/posts.json")
          .then(function(data){
            return data.json();
          })
           .then(function (data) {
            var blogsArray = [];
            for(let key in data){
              data[key].id = key;
              blogsArray.push(data[key]);
            }
            this.customers = blogsArray;
      })
        },
        filterBy(customers,value){
            return customers.filter(function(customer){
              return customer.name.match(value);
            })
        }
    },
    created(){
        if (this.$route.query.alert) {
            this.alert = this.$route.query.alert;
        }
        this.fetchCustomers();
    },
    updated(){
        this.fetchCustomers();
    },
    components:{
        Alert
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
