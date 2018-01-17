<template>
  <div class="cars container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1 class="page-header">Manage Cars</h1>
    <input class="form-control" placeholder="Enter Last Name" v-model="filterInput">
    <br />
    <table class="table table-striped">
        <thead>
          <tr>
            <th>Marca</th>
            <th>Model</th>
            <th>Unitati</th>
            <th>Pret</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="car in filterBy(cars, filterInput)">
            <td>{{car.maker}}</td>
            <td>{{car.model}}</td>
            <td>{{car.tot_count}}</td>
            <td>{{car.price}}</td>
            <td><router-link class="btn btn-default" v-bind:to="'/car/'+car.id">View</router-link></td>
          </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
  import Alert from './Alert';
  export default {
    name: 'cars',
    data () {
      return {
        cars: [],
        alert:'',
        filterInput:''
      }
    },
    methods: {
      fetchCars(){
        this.$http.get('http://local.slimapp.ro/api/cars')
          .then(function(response){
            this.cars = response.body;
          });
      },
      filterBy(list, value){
        value = value.charAt(0) + value.slice(1);
        return list.filter(function(car){
          return car.model.indexOf(value) > -1;
        });
      }
    },
    created: function(){
      if(this.$route.query.alert){
        this.alert = this.$route.query.alert;
      }
      this.fetchCars();
    },
    updated: function(){
      this.fetchCars();
    },
    components: {
      Alert
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
