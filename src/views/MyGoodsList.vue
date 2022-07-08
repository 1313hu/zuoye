<template>
    <div>
   <MyTable :arr="list">
    <template #header> 
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
    </template>
    <template #tboody="scope">
        <td>{{ scope.row.id }}</td>
        <td>{{ scope.row.goods_name }}</td>
        <td>{{ scope.row.goods_price }}</td>
        <!-- <td>{{ scope.row.tags }}</td> -->
        <td>

            <input
            type="text" 
          class="tag-input form=control"
            style="width: 100px"
            v-focus

            v-if="scope.row.inputShow"
           
            />
            <button
            class="btn btn-primary btn-sm add-tag"
            v-else
            @click="scope.row.inputShow = true">
            +Tag
     </button>
            <span
            style="margin-right: 8px"
            class="badge badge-warning"
            v-for="(item,index) in scope.row.tags"
            :key="index">
            {{item}} 
            </span>
          <button class="btn btn-danger btn-sm" @click="del(scope.row.id)">删除</button>
        </td>
        </template>

   </MyTable>
    </div>
</template>

<script>
import MyTable from "../components/MyTable.vue"

export default {
    
    name: 'TabbarMyGoodsList',
    components: {
        MyTable,
    },

    data() {
        return {
            list: []
        };
    },
    created() {
       this.$axios({
            url: "/api/goods",
        }).then((res) => {
            // console.log(res.data.data);
            res.data.data.forEach((ele) => {
                ele.inputshow =false;
            })
            // console.log(res.data.data);
            this.list = res.data.data
            console.log(this.list);
        });    },

    methods: {
       del(id) {
        const index = this.list.findIndex((ele) => ele.id ==id);
        this.list.splice(index,1)
       },
       enterFn(obj) {
        console.log(obj.inputValue,"===",obj.tags);
        if (obj.inputValue.trim() == "") {
            return alert("enter message")
        }
        obj.tags.push(obj.inputValue);
        obj.inputValue = ""
       }
       
    }
    
}
   

   
</script>

<style lang="scss" scoped>

</style>