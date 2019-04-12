<!DOCTYPE html>
<html>
<style>
#myForm {
  transition: all linear 0.5s;
  height:auto;
  width: 100%;
  top: 0;
  left: 0;
}

.ng-hide {
  height: 0;
  width: 0;
  background-color: transparent;
  top:-200px;
  left: 200px;
}
</style>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.9/angular.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>

<!-- angularjs -->
<script>
var app = angular.module('app', []);
app.controller('formControl', ($scope, $http) => {
      $scope.updating = false;
      
      $scope.create = (name,city,country) => {
          customer = { 
            Name: name, 
            City: city,
            Country: country
          };
          $scope.customers.push(customer);
      }
      
     $scope.addCustClick = () => {
     	$scope.showCust = !$scope.showCust;
     }
        
     $http.get("customers.php").then((response) => {
          $scope.customers = response.data.records;
      });
    
      $scope.delete = (c) => {
          $scope.customers.pop(c.$$hashkey)
      }
      
      $scope.update = (c) => {
      	$scope.updating = !$scope.updating;
      }
});
</script>
<!-- endof angularjs -->


<body class="container" style="width:100%">
<div class="jumbotron" style="margin-top:2.5%;margin-bottom:3%;background-color:black;color:white;text-align:center">
  <h3>Bootstrap & AngularJS Tutorial</h3> 
</div>
<div ng-app="app" ng-controller="formControl" style="border:3px solid black;padding:20px;border-radius:15px">
<h5 style="text-align:center">Filter Table</h5>
<input type="text" placeholder="Filter term..." class="form-control" ng-model="inputFilter">

<table class="table" style="border-bottom: 1px lightgray solid">
	<th>Customer Name</th>
    <th>Customer Location</th>
    <th>Update</th>
    <th>Remove</th>
	<tr ng-init="get()" ng-repeat="c in customers | filter: inputFilter | limitTo: 5">
      <td ng-show="!updating">{{ c.Name }}</td>
      <td ng-show="!updating">{{ c.City + ',' + c.Country}}</td>
      
      <td ng-show="updating">
      	<input type="text" class="form-control" ng-model="c.Name">
	  </td>
      <td ng-show="updating">
        <div>
          <input type="text" class="form-control" ng-model="c.City" placeholder="City">
          <input type="text" class="form-control" ng-model="c.Country" placeholder="Country">
        </div>
      </td>
          
       <td><button ng-click="update(c)" class="btn btn-outline-primary btn-sm" style="background-color:orange;color:white;border: 0"> Update </button></td>
      <td><button ng-click="delete(c)" class="btn btn-outline-primary btn-sm" style="background-color:red;color:white;border: 0"> Remove </button></td>

  </tr>
</table>

<button ng-hide="showCust" ng-click="addCustClick()" class="btn btn-primary btn-sm" style="background-color:#00cc00;color:white;border:0;margin-left:38%"> Show Customer Form</button>
<button ng-show="showCust" ng-click="addCustClick()" class="btn btn-primary btn-sm" style="background-color:#00cc00;color:white;border:0;margin-left:38%"> Hide Customer Form</button>

<form ng-show="showCust" class="form-group" id="myForm">
<h5 style="margin-top:15px;">Add New Customer</h5>
  <p>Name: <input type="text" class="form-control" ng-model="name" required></p>
  <p>City: <input type="text" class="form-control" ng-model="city" required></p>
  <p>Country: <input type="text" class="form-control" ng-model="country" required></p>
  <button class="btn btn-primary btn-sm btn-block" style="background-color:#00cc00;color:white;border:0" ng-click="create(name,city,country)"> Add </button>
</form>


</body>
</html>
