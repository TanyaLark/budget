<template>
  <div id="app">
    <Form @submitForm="onFormSubmit"/>
    <TotalBalance :total="totalBalance" />
      <div class="sortList">
        <el-button type="success" icon="el-icon-top" @click="showOnlyIncome()">Show only income</el-button>
        <el-button type="info" icon="el-icon-s-data" @click="showAll()">Show all</el-button>
        <el-button type="danger" icon="el-icon-bottom" @click="showOnlyOutcome()">Show only outcome</el-button>
      </div>
    <BudgetList :list="sortList" @deleteItem="onDeleteItem" />
  </div>
</template>

<script>
import BudgetList from "@/components/BudgetList";
import TotalBalance from "@/components/TotalBalance";
import Form from "@/components/Form";

export default {
  name: 'App',
  components: {
    BudgetList,
    TotalBalance,
    Form
  },
  data: () => ({
    list: {
      1: {
        type: "INCOME",
        value: 100,
        comment: "Some comment",
        id: 1
      },
      2: {
        type: "OUTCOME",
        value: -50,
        comment: "Some outcome comment",
        id: 2
      }
    },
    sortList: {}
  }),
  computed: {
    totalBalance(){
      Object.values(this.list).map(function(el) {
         if (el.type === "OUTCOME" && el.value > 0 ){
            return el.value = 0 - el.value;
          }
      });

      return Object.values(this.list).reduce(
        (acc, item) => acc + item.value,0
      );
    }
  },
  methods:{
    onDeleteItem(id){
      this.$delete(this.sortList, id);
      this.$delete(this.list, id);
    },
    onFormSubmit(data){
      const newObj = {
        ...data,
        id: String(Math.random())
      };

      this.$set(this.list, newObj.id, newObj);
    },
    showOnlyIncome(){
      const incomeList = Object.values(this.list).filter(item => item.value > 0);
      const entriesArr = [];
      for (let i = 0; i < incomeList.length; i++) {
        const keysArray = [incomeList[i].id, incomeList[i]];
        entriesArr.push(keysArray)
       }
      const entries = new Map(entriesArr);
      this.sortList = Object.fromEntries(entries);
      return this.sortList;
    },
    showAll(){
      return this.sortList = this.list;
    },
    showOnlyOutcome(){
      const outcomeList = Object.values(this.list).filter(item => item.value < 0);
      const entriesArr = [];
      for (let i = 0; i < outcomeList.length; i++) {
        const keysArray = [outcomeList[i].id, outcomeList[i]];
        entriesArr.push(keysArray)
       }
      const entries = new Map(entriesArr);
      this.sortList = Object.fromEntries(entries);
      return this.sortList;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
