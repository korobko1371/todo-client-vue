<template>
  <div id="app">
    <router-view/>
  </div>

</template>

<script>
export default {
  name: 'App'
}

</script>

<style lang="scss">
  @import "../node_modules/bootstrap/dist/css/bootstrap.css";

  h2{
    text-align: center;
  }
  body{

  }
  .card {
    -webkit-box-shadow: 8px 10px 4px -7px rgba(214,32,214,1);
    -moz-box-shadow: 8px 10px 4px -7px rgba(214,32,214,1);
    box-shadow: 8px 10px 4px -7px rgba(214,32,214,1);
    width: 300px;
    height: 350px;
    margin: 10px;
    border: 1px solid black;
    padding: 10px;
    background: #fbe8ff
  }
  #addCategory{
    width: 100px;
    height: 100px;
    margin: 120px 90px;
    top: 150px;


    -webkit-transition: -webkit-transform .01s ease-in-out;
    transition:         transform .01s ease-in-out;
  }
  #addCategory:hover{

  -webkit-transform: rotate(10deg);
          transform: rotate(10deg);
  }

  #deleteCategory{
    width:30px;
    height:30px;
    position: absolute;
    right: 0;
    top: 0;
  }
  #addBusiness{
    width:30px;
    height:30px;
    position: absolute;
    left: 0;
    bottom: 0;
  }


  .modal-mask-business {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.1);
    display: table;
    transition: opacity .3s ease;
  }
  .modal-mask-category {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.1);
    display: table;
    transition: opacity .3s ease;
  }
  .modal-wrapper {
    display: table-cell;
    vertical-align: middle;
  }
  ul {
    padding: 0; margin: 0; /* Убираем отступы у списка */
  }
  li {
    list-style-type: none; /* Убираем маркеры */
  }
  .strike {
    text-decoration: line-through;
    color: #aaa;
  }
</style>
