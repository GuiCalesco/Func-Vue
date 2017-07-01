<template>
  <a class="fixo button is-large is-danger is-loading" v-show="isLoading">Loading</a>
  <div class="container">
    <h1 class="title">{{title}}</h1>
    <div class="columns">
      <div class="column is-5">
        <p class="control has-addons">
          <input class="input is-expanded" type="text" placeholder="Procure pelo nome" v-model="search"><a class="button is-info" @click="searchFuncionarios">Search</a>
        </p>
      </div>
    </div>
    <div class="column is-5">

      </div>
    </div>
    <div class="columns">
      <div class="column is-12">
        <table class="table is-narrow is-bordered">
          <thead>
            <th>Id</th>
            <th>Nome</th>
            <th>Cargo</th>
            <th>Salario</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="funcionario in funcionarios">
            <td>{{funcionario.id}}</td>
            <td>{{funcionario.nome}}</td>
            <td>{{funcionario.cargo}}</td>
            <td>R${{funcionario.salario}},00</td>
            <td class="is-icon">
              <a href="#">
                <i class="fa fa-edit"></i>
              </a>
              <a href="#">
                <i class="fa fa-trash"></i>
              </a>
            </td>
          </tr>
        </tbody>
      </table>
  </div>
</div>
</template>
<script>


  export default {
    data () {
      return {
        isLoading: false,
        title: 'Funcionarios da Empresa',
        search: '',
        funcionarios: [],
        page: 1,
        total: 0,
        selected: {},
        itensPerPage: 100
      }
    },
    methods: {
      showLoading(){
        this.isLoading=true;
      },
      hideLoading(){
        this.isLoading=false;
      },
      loadfuncionarios(){

        let t = this
        this.showLoading()

        let start = (this.page * this.itensPerPage) - (this.itensPerPage)
        let end = this.page * this.itensPerPage

        this.$http.get(`/funcionarios?_start=${start}&_end=${end}`).then(
         response=>{
           t.funcionarios = response.body
           t.total = response.headers.get['X-Total-Count']
         },
         error=>{
           console.log(error)
         }).finally(function () {
          t.hideLoading();
        })

       },
       searchFuncionarios(){

       }
     },
     created(){
      this.loadfuncionarios();
    },
  }
</script>
<style>
  .fixo{float: right; margin-right: 10px; margin-top: 0px; z-index: 1000;}
</style>
