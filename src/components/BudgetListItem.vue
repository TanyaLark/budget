<template>
  <div>
    <div class="list-item" v-for="(item, toItem) in getItem" :key="toItem">
      <span class="budget-comment">{{ item.comment }}</span>
      <span class="budget-value green" v-if="item.value > 0">{{ item.value }}
        <i class="el-icon-top"></i>
      </span>
      <span class="budget-value red" v-else>{{ item.value }}
        <i class="el-icon-bottom"></i>
      </span>
      <ElButton type="danger" size="mini" @click="deleteDialog(item.id)">Delete</ElButton>

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
  }),
  props: {
   toItem:{
     type: Object,
     default: () => ({})
   },
 },
  computed:{
    getItem(){
      return this.toItem;
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
    }
  }
}

</script>

<style scoped>

.list-item{
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
