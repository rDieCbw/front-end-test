<template>
  <v-layout column >
    <v-flex>
      <h1>Pessoas que vão ganhar dinheiro</h1>
      <div class="list">
        <v-list three-line v-if="employees">
          <template v-for="employee in employees">
            <v-list-item :key="employee.id" class="employee-card">
              <div class="status-badge" :class="cssBadgeType(employee.progress)">
                <img src="@/assets/img/ico-union.svg"> {{ cssBadgeText(employee.progress) }} <span class="value" v-if="employee.progress > 0">{{ Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(employee.progress) }}</span>
              </div>
              <v-list-item-avatar class="d-none d-md-block">
                <img src="@/assets/img/union.svg">
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-title v-html="employee.employee_name"></v-list-item-title>
                <v-list-item-subtitle>Ao clicar no link abaixo, uma dialog irá aparecer perguntando quantos reais você deseja adicionar a barra de progresso. A barra deve começar em 0.</v-list-item-subtitle>
                <div class="progress-bar">
                  <div class="progress-max" >Total R$ 200,00</div>
                  <div class="progress" :style="cssProgressWidth(employee.progress)"></div>
                  <div class="progress-value" :style="cssProgressValueWidth(employee.progress)">{{ Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(employee.progress) }}</div>
                </div>
                <v-btn text @click.stop="showModalMoney(employee.id)"><img src="@/assets/img/ico-add.svg"> Clique aqui para adicionar reais</v-btn>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-list>
        <div class="loading" v-else>
          <v-progress-circular indeterminate color="#059D42" ></v-progress-circular>
          Carregando Funcionários
        </div>
      </div>
    </v-flex>
    <v-dialog v-model="addMoneyDialog" max-width="600">
      <v-card>
        <v-card-title class="headline">Quantos reais adicionar para {{ (contextEmployeeIndex != null) ? employees[contextEmployeeIndex].employee_name : '' }}? <button class="close-modal" @click="addMoneyDialog = false">X</button></v-card-title>
        <v-card-text>
          <v-layout>
            <button class="add-money-button" @click="addMoney(25)" > R$ 25 </button>
            <button class="add-money-button" @click="addMoney(50)" > R$ 50 </button>
          </v-layout>
          <v-layout>
            <button class="add-money-button" @click="addMoney(75)" > R$ 75 </button>
            <button class="add-money-button" @click="addMoney(125)" > R$ 125 </button>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
const axios = require('axios');
  export default {
    data() {
      return{
        employees: null,
        addMoneyDialog: false,
        contextEmployeeIndex: null,
      }
    },
    async created() {
      this.getEmployees();
    },
    methods: {
      getEmployees() {
        var self = this;
        axios.get('https://dummy.restapiexample.com/api/v1/employees')
          .then(function (response) {
            var employeesRequest = response.data.data.map((employee) => {
              employee.progress = 0;
              return employee;
            });
            self.employees = employeesRequest;
        })
        .catch(function (error) {
          self.getEmployees();
        });
      },
      cssBadgeType(progress) {
        return (progress <= 0) ? 'red' : 'green';
      },
      cssBadgeText(progress) {
        return (progress <= 0) ? 'Você não adicionou nada' : 'Você já adicionou ';
      },
      cssProgressValueWidth(value){
        if(value > 0 && value <=200){
          return 'color:white; width:'+((value*100)/200) + '%';
        }
      },
      cssProgressWidth(value){
        if(value <= 200){
          return 'width:'+((value*100)/200) + '%';
        } 
      },
      showModalMoney(employee_id) {
        //THIS IS NOT WORKING
        this.addMoneyDialog = true;
        this.setContexEmployeeIndex(employee_id);
        console.log(this.employees[this.contextEmployeeIndex].employee_name)
      },
      addMoney(value) {
        if(this.employees[this.contextEmployeeIndex].progress + value <= 200){
          this.employees[this.contextEmployeeIndex].progress+= value;
        }
        this.addMoneyDialog = false;
        this.contextEmployee = null;
      },
      setContexEmployeeIndex(employee_id){
        for (let i = 0, j = this.employees.length; i < j; i++) {
          if(this.employees[i].id == employee_id){
            this.contextEmployeeIndex = i;
          }
          break;
        }
      }
    },
  }
</script>