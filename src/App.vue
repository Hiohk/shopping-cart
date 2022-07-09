<template>
  <div class="app-container">
    <MyHeader title="购物车"></MyHeader>
    <Goods
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :state="item.goods_state"
      :count="item.goods_count"
      @state-change="getNewState"
    >
      <Counter
        :num="item.goods_count"
        :id="item.id"
        @num-change="getNewNum(item, $event)"
      ></Counter>
    </Goods>
    <Footer
      :isfull="fullState"
      :amount="amount"
      :all="total"
      @full-change="getFullState"
    ></Footer>
  </div>
</template>

<script>
import axios from "axios";
import bus from "@/components/eventBus.js";
import MyHeader from "@/components/Header/Header.vue";
import Goods from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";
import Counter from "@/components/Counter/Counter.vue";

export default {
  components: {
    MyHeader,
    Goods,
    Footer,
    Counter,
  },
  data() {
    return {
      list: [],
    };
  },
  created() {
    this.initCartList();
    bus.$on("share", (val) => {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_count = val.value;
          return true;
        }
      });
    });
  },
  computed: {
    //动态计算出全选的状态
    fullState() {
      return this.list.every((item) => item.goods_state);
    },
    //选中的商品总价格
    amount() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((total, item) => (total += item.goods_price * item.goods_count), 0);
    },
    //选中的商品总数量
    total() {
      return this.list
        .filter((item) => item.goods_state)
        .reduce((total, item) => (total += item.goods_count), 0);
    },
  },
  methods: {
    async initCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    getNewState(val) {
      this.list.some((item) => {
        if (item.id === val.id) {
          item.goods_state = val.value;
        }
      });
    },
    getFullState(val) {
      this.list.forEach((item) => (item.goods_state = val));
    },
    getNewNum(val, e) {
      val.goods_count = e;
    },
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
  height: 100%;
}
</style>
