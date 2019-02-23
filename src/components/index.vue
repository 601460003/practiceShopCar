<template>
  <div class="app" style="text-align: center">
    <!--中间的列表-->
    <div class="main_list">
      <div v-for="shop in list">
        <input type="checkbox" :checked="shop.checked" @click="singerShopSelect(shop)">
        <img src="http://life.southmoney.com/tuwen/UploadFiles_6871/201803/20180329163419413.jpg" width="100">
        <span>{{shop.price|moneyFrom(shop.price)}}</span>
        <button @click="addCount(shop,-1)">-</button>
        <input type="text" value="shop.count" v-model="shop.count" style="width: 10px">
        <button @click="addCount(shop,+1)">+</button>
        <span>单品总金额：</span>
        <span style="color: red">{{shop.price*shop.count|moneyFrom(shop.price*shop.count)}}</span>
        <button @click="shanchu(shop)">X</button>
      </div>
      <div style="width: 200px;height: 200px;background: red;position: absolute;right: 800px;top: 100px;"
           :class="{yincang:yincang}">
        <button @click="delshangpin( )">确定</button>
        <button @click="yincang=true">取消</button>
      </div>
      <!--全选方法-->
      <div>
        <input type="checkbox" :checked="ischecked" @click="AllChecked(ischecked)">
        <span>总金额:</span>
        <span>{{totalMoney|moneyFrom(totalMoney)}}</span>

      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Index",

    data() {

      return {
        //判断是否选中
        ischecked: false,
        //所有商品的总价
        totalMoney: 0,
        yincang: true,
        shangchuduixiang: {},
        list: [
          {
            shopid: 1,
            price: 299,
            count: 1,
          },
          {
            shopid: 2,
            price: 1699,
            count: 1,
          },
          {
            shopid: 3,
            price: 999,
            count: 1,
          },
        ]
      }
    },
    //第一步操作添加 过滤器
    filters: {
      moneyFrom(money) {
        return '￥' + money.toFixed(2)
      },
    },
    methods: {
      //第二部操作给加减添加事件 第一步判断哪个是加那个是减
      addCount(shop, way) {
        if (way > 0) {
          shop.count++;
        } else {
          if (shop.count <= 1) {
            shop.count = 1;
            return;
          }
          shop.count--;
        }
        this.getALLshopPrice();
      },
      //第三部操作，选定所有的商品
      AllChecked(e) {
        //总控制
        this.ischecked = !e;
        //需要控制其他东西的时候就要遍历所有的东西
        this.list.forEach(a => {
          if (typeof a.checked === 'undefined') {
            this.$set(a, 'checked', !e)
          }
          else {
            a.checked = !e;
          }
        });
        this.getALLshopPrice();

      },
      //第四部操作计算商品的总价格
      getALLshopPrice() {
        this.totalMoney = 0;
        this.list.forEach(e => {
          //判断商品是否被选中的时
          if (e.checked) {
            this.totalMoney += e.price * e.count;
          }
        });
      },
      //第五步操作单个商品选中或这取消
      singerShopSelect(a) {
        if (typeof a.checked === 'undefined') {
          this.$set(a, 'checked', true)
        } else {
          a.checked = !a.checked;
        }
        this.getALLshopPrice();
        this.hasSelectAll();

      },
      //第六步判断是否全选的方法
      hasSelectAll() {
        let flag = true;
        this.list.forEach(e => {
          if (!e.checked) {
            flag = false;
          }
        });
        this.ischecked = flag;
      },
      //第七部 删除商品
      shanchu(shop) {
        this.yincang = false;
        this.shangchuduixiang = shop;
      },
      //第八删除当前的商品
      delshangpin() {
        this.yincang = true;
        let index = this.list.indexOf(this.shangchuduixiang);
        this.list.splice(index, 1);
        this.getALLshopPrice();
      }
    },
  }
</script>

<style scoped>
  .yincang {
    display: none;
  }
</style>
