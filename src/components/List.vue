<template>
    <div class="container">
        <div class="row">
            <div v-for="category in list">
                <div class="col-sm-12 col-xs-12 col-md-6 col-lg-4">
                    <div class="card">
                        <h2>{{category.name}}</h2>
                        <div v-for="business in category.business">
                            <ul>
                                <li v-bind:class="{'strike': business.isCompleted}">
                                    <input type="checkbox" v-model="business.isCompleted"  @click="changeStatus(business._id,!business.isCompleted)">
                                    {{business.name}}
                                </li>
                            </ul>
                        </div>
                        <img id="deleteCategory" src="../assets/images/trash_bin.png" v-on:click="deleteCategory(category.name)">
                        <img id="addBusiness" @click="showModal_business = true; clickedCard = category.name" src="../assets/images/add.png">
                        <!-- Modal Добавление нового дела -->
                        <div v-if="showModal_business">
                            <transition name="modal">
                                <div class="modal-mask-business">
                                    <div class="modal-wrapper">

                                        <div class="modal-dialog" role="document">
                                            <div class="modal-content">
                                                <div class="modal-header">
                                                    <h5 class="modal-title">Введите название нового дела</h5>

                                                </div>
                                                <div class="modal-body">

                                                  <div> <input @keyup.enter="addBusinessIntoCategory(clickedCard);" type="text" class="form-control" v-model="newBusiness" /></div>
                                                  <p style="color: #ff0000">{{alert}}</p>
                                                </div>
                                                <div class="modal-footer">
                                                    <button type="button" class="btn btn-secondary" @click="showModal_business = false;alert = null">Отмена</button>
                                                    <button type="button" class="btn btn-primary" @click="addBusinessIntoCategory(clickedCard);" >Добавить дело</button>

                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </transition>
                        </div>
                        <!-- Modal-->
                    </div>
                </div>
            </div>
            <div class="col-sm-12 col-xs-12 col-md-6 col-lg-4">
                <div class="card" @click="showModal = true">
                    <img id="addCategory" src="../assets/images/add.png">
                </div>

                <!-- Modal Добавление новой категории-->
                <div v-if="showModal">
                    <transition name="modal">
                    <div class="modal-mask-category">
                        <div class="modal-wrapper">

                            <div class="modal-dialog" role="document">
                                <div class="modal-content">
                                    <div class="modal-header">
                                        <h5 class="modal-title">Введите название новой категории</h5>

                                    </div>
                                    <div class="modal-body">
                                        <input @keyup.enter="addCategory(newCategory);" type="text" class="form-control" v-model="newCategory" />
                                        <p style="color: #ff0000">{{alert}}</p>
                                    </div>
                                    <div class="modal-footer">
                                        <button type="button" class="btn btn-secondary" @click="showModal = false; newCategory = null; alert = null;">Отмена</button>
                                        <button type="button" class="btn btn-primary" @click="addCategory(newCategory);" >Добавить новую категорию</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    </transition>
                </div>
                <!-- Modal-->

            </div>
        </div>
    </div>

</template>

<script>
    import axios from 'axios';
    export default {
        name: 'ListPage',
        data () {
            return {
                baseUrl: process.env.baseURL || "https://todo-list123456.herokuapp.com",
                list: [],
                showModal: false,
                showModal_business: false,
                newCategory: "",
                newBusiness: "",
                clickedCard: null,
                alert: ""
            }
        },
        methods: {
            fetchingData: function(){
                axios
                .get(`${this.baseUrl}/categories`)
                .then(response => (this.list = response.data))
                .catch(err => console.log(err));
            },
            nameCheck: function(nc){
              for (let i = 0; i< this.list.length; i++){
                if (this.list[i].name == nc){
                  return true;
                }
              }
              return false;
            },
            addCategory: function(){
              if (this.newCategory == "" | this.newCategory == null) {
                this.alert = "Не может быть пустым";
              } else if (this.nameCheck(this.newCategory)){
                this.alert = "Уже имеется!";
              }else{
                axios
                .post(`${this.baseUrl}/categories`, {name: this.newCategory})
                .then(res => this.fetchingData())
                .catch(err => console.log(err));
                this.newCategory = "";
                this.showModal = false;
                this.alert = "";
              }
            },
            deleteCategory: function(categoryName){
                axios
                .delete(`${this.baseUrl}/categories`,{data: {name : categoryName}})
                .then(res => this.fetchingData())
                .catch(err => console.log(err));
            },
            addBusinessIntoCategory: function(categoryName){
              if (this.newBusiness == "" | this.newBusiness == null) {
                this.alert = "Не может быть пустым";
              } else {
                axios
                .post(`${this.baseUrl}/categories/`+categoryName, {name: this.newBusiness, isCompleted: false})
                .then(res => this.fetchingData())
                .catch(err => console.log(err));
                this.newBusiness = "";
                this.showModal_business = false;
                this.alert = "";
              }
            },
            changeStatus: function(id, status){
                axios
                .put(`${this.baseUrl}/business/`+id,{status: status})
                .then(res => this.fetchingData())
                .catch(err => console.log(err));
            }

        },
        created: function() {
            this.fetchingData();
        }
    }
</script>
