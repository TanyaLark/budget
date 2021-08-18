<template>
  <div class="budget-list-wrap">

    <ElCard :header="header">
      <!-- <div class="sortList">
        <el-button type="success" @click="showOnlyIncome()">Show only income</el-button>
        <el-button type="info" @click="showAll()">Show all</el-button>
        <el-button type="danger" @click="showOnlyOutcome()">Show only outcome</el-button>
      </div> -->
      <template v-if="!isEmpty">
        <BudgetListItem :toItem="toItem" @deleteItem="onDeleteItem"/>
      </template>
      <ElAlert v-else type="info" :title="emptyTitle" :closable="false" />
    </ElCard>
  </div>
</template>

<script>
import BudgetListItem from "./BudgetListItem.vue";

export default {
 name: "BudgetList",
 components: {
   BudgetListItem,
 },
 props: {
   list: {
     type: Object,
     default: () => ({})
   }
 },
  data: () => ({
    header: "Budget List",
    emptyTitle: "Empty List",
    sortList: {}
  }),
  computed:{
    isEmpty(){
      return !Object.keys(this.list).length;
    },
    toItem(){
      return this.list;
    }
  },
  methods:{
    onDeleteItem(id){
      this.$emit('deleteItem', id);
    },
    showOnlyIncome(){
      const incomeList = Object.values(this.list).filter(item => item.value > 0);
      this.sortList = Object.assign({}, incomeList);
      return this.sortList;
    },
    showAll(){
      return this.sortList = this.list;
    },
    showOnlyOutcome(){
      const outcomeList = Object.values(this.list).filter(item => item.value < 0);
      this.sortList = Object.assign({}, outcomeList);
      return this.sortList;
    }
  }
}
</script>

<style scoped>

.budget-list-wrap{
  max-width: 500px;
  margin: auto;
}

</style>
