<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from "@/components/eventBus.js";

export default {
  props: {
    // 接收商品的 id 值，将来，使用 EventBus 方案，
    // 把数量传递到 App.vue 的时候，需要通知 App 组件，更新哪个商品的数量
    id: {
      type: Number,
      required: true,
    },
    // 接收到的 num 数量值
    num: {
      type: Number,
      default: 1,
    },
  },
  components: {},
  data() {
    return {};
  },
  methods: {
    sub() {
      if (this.num - 1 === 0) return;
      this.$emit("num-change", this.num - 1);
      // const obj = {
      //   id: this.id,
      //   value: this.num - 1,
      // };
      // bus.$emit("share", obj);
    },
    add() {
      // const obj = {
      //   id: this.id,
      //   value: this.num + 1,
      // };
      this.$emit("num-change", this.num + 1);
    },
  },
};
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
