<template>
  <div class="counter-container">
    <!-- 数量 -1 按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="onSubClick">-</button>
    <!-- 输入框 -->
    <input type="number" class="form-control form-control-sm ipt-num" v-model.number.lazy="number" > </input>
    <!-- 数量 +1 按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="onAddClick">+</button>
  </div>
</template>

<script>
export default {
  name: "EsCounter",
  props: {
    num: {
      type: Number,
      default: 0,
    },
    min: {
      type: Number,
      default: NaN,
    },
  },
  // data中的值是可读可写的，props中的值只可读，如果要改变props中的值，可以把这个值转存入data中
  data() {
    return {
      number: this.num,
    };
  },
  emits:['numChange'],
  methods: {
    onSubClick() {
      // 如果传过来的值不是NaN，同时
      if (!isNaN(this.min) && this.number - 1 < this.min) return;
      this.number--;
    },
    onAddClick() {
      this.number++;
    },
  },
  watch: {
    // 用侦听器对输入的值做一个判断
    number(newVal) {
      const parseResult = parseInt(newVal);
      if (isNaN(parseResult) || parseResult < 1) {
        this.number = 1;
        return;
      }
      // 把新值转成字符串，并且用indexOf方法查看是否包含小数点，如果包含了就执行下面的
      if (String(newVal).indexOf(".") !== -1) {
        // 包含了
        this.number = parseResult;
        return;
      }
      // console.log(this.number);
      // 触发自定义事件，把最新的number数值传递给组件的使用者
      this.$emit('numChange', this.number);
    },
  },
};
</script>

<style lang="less" scoped>
.counter-container {
  display: flex;
  // 按钮的样式
  .btn {
    width: 25px;
  }
  // 输入框的样式
  .ipt-num {
    width: 34px;
    text-align: center;
    margin: 0 4px;
  }
}
</style>