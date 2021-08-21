<template>
  <div>

    <div class="filterBtns">
      <el-button type="success" icon="el-icon-top" @click=" showOnlyIncome()"> Only income</el-button>
      <el-button type="info" icon="el-icon-s-data" @click=" showAll()"> Show all</el-button>
      <el-button type="danger" icon="el-icon-bottom" @click=" showOnlyOutcome()"> Only outcome</el-button>
    </div>

    <div class="list-items" v-for="(item, changeList) in toItem" :key="changeList">

      <template v-if="selectedType === 'Income'">
        <div class="item" v-show="item.value > 0">
          <span class="budget-comment">{{ item.comment }}</span>
          <span class="budget-value green" v-if="item.value > 0">{{ item.value }}
            <i class="el-icon-top"></i>
          </span>
          <span class="budget-value red" v-else>{{ item.value }}
            <i class="el-icon-bottom"></i>
          </span>
          <ElButton type="danger" size="mini" @click="deleteDialog(item.id)">Delete</ElButton>
        </div>
      </template>

      <template v-else-if="selectedType === 'Outcome'">
        <div class="item" v-show="item.value < 0">
          <span class="budget-comment">{{ item.comment }}</span>
          <span class="budget-value green" v-if="item.value > 0">{{ item.value }}
            <i class="el-icon-top"></i>
          </span>
          <span class="budget-value red" v-else>{{ item.value }}
            <i class="el-icon-bottom"></i>
          </span>
          <ElButton type="danger" size="mini" @click="deleteDialog(item.id)">Delete</ElButton>
        </div>
      </template>

      <template v-else>
        <div class="item" v-show="item">
          <span class="budget-comment">{{ item.comment }}</span>
          <span class="budget-value green" v-if="item.value > 0">{{ item.value }}
            <i class="el-icon-top"></i>
          </span>
          <span class="budget-value red" v-else>{{ item.value }}
            <i class="el-icon-bottom"></i>
          </span>
          <ElButton type="danger" size="mini" @click="deleteDialog(item.id)">Delete</ElButton>
        </div>
      </template>

      <ElDialog
        title="Warning"
        :visible.sync="centerDialogVisible"
        width="30%"
        center>
        <span> Do you want to delete item? </span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="centerDialogVisible = false">Cancel</el-button>
          <el-button type="primary" @click="deleteItem()"> Yes </el-button>
        </span>
      </ElDialog>

    </div>
  </div>
</template>

<script>

export default {
  name: "BudgetListItem",
  data: () => ({
    centerDialogVisible: false,
    id: 0,
    changeList: {},
    selectedType: 'All',
  }),
  props: {
   toItem:{
     type: Object,
     default: () => ({})
   },
  },
  methods: {
    deleteDialog(id) {
      this.centerDialogVisible = true;
      this.id = id;
      return id;
    },
    deleteItem(){
      this.$emit('deleteItem', this.id);
      this.centerDialogVisible = false;
      this.$delete(this.changeList, this.id);
    },
    showOnlyIncome(){
      return  this.selectedType = "Income";
    },
    showAll(){
      return this.selectedType = "All";
    },
    showOnlyOutcome(){
      return this.selectedType = "Outcome";
    }
  }
}

</script>

<style scoped>

.item{
  display: flex;
  align-items: center;
  padding: 10px 15px;
}

.budget-value{
  font-weight: bold;
  margin-left: auto;
  margin-right: 20px;
}

.green{
  color:green;
}

.red{
  color: red;
}

</style>
