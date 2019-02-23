<template>
  <div class="hello">
    <div v-for="fruit in list">
      <input type="checkbox" v-model="checkData" :value="fruit.name">

      <span>{{fruit.price}}</span>
      <span style="margin-left: 20px;width: 120px">{{fruit.name}}</span>


    </div>
    <div>price:￥{{totalPrice}}</div>
    <div>
      <label>全选</label><input type="checkbox" @click="checkAll" :checked="checkedAll">
    </div>
  </div>
</template>

<script>
  export default {
    name: "shopcar",
    data() {
      return {
        checkData: [],// 双向数据绑定的数组 用来保存选择的值
        list: [
          {
            name: 'apple',
            price: 8,
            checked: false
          },
          {
            name: 'Sydney',
            price: 5,
            checked: false
          },
          {
            name: 'banana',
            price: 3,
            checked: false
          },
        ],
      }
    },
    watch: {// 监视双向绑定的数组变化
      checkData: {
        handler() {//默认写法
          if (this.checkData.length != this.list.length) {
            this.checkedAll = false;
          } else {
            this.checkedAll = true;
          }

          //计算金额
          var totalPrice = 0;
          this.list.forEach(e => {
            //双重循环
            //['apple','banana']
            this.checkData.forEach(o => {
              if (e.name == o)
                totalPrice += e.price;
            })
          })
          this.totalPrice = totalPrice;
        }

      }
    },
    methods: {
      checkAll() {
        //设置全选框
        this.checkedAll = !this.checkedAll;
        //先数组置空
        this.checkData = [];

        var totalPrice = 0;
        if (this.checkedAll) {
          this.list.forEach(e => {
            totalPrice += e.price;
            this.checkData.push(e.name);
          })
        }
        this.totalPrice = totalPrice;
      },
    }
  }
</script>

<style scoped>

</style>
