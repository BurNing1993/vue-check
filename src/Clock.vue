<template>
  <div class="time">
    <span>{{ hour }}</span>
    <span v-show="show" class="interval"> : </span>
    <span v-show="!show" class="interval"> </span>
    <span>{{ min }}</span>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted, ref } from "vue";
import dayjs from "dayjs";

export default defineComponent({
  name: "Clock",
  setup() {
    let show = ref(true);
    let min = ref("");
    let hour = ref("");
    const getTime = () => {
      min.value = dayjs().format("mm");
      hour.value = dayjs().format("HH");
    };
    getTime();
    let timer = null;
    onMounted(() => {
      timer = setInterval(() => {
        getTime();
        show.value = !show.value;
      }, 1000);
    });
    onUnmounted(() => {
      if (timer) {
        clearInterval(timer);
      }
    });
    return {
      min,
      hour,
      show,
    };
  },
});
</script>

<style scoped>
.time {
  font-size: 22px;
  color: rgb(3, 3, 3);
  font-weight: 550;
  display: flex;
  align-items: center;
  height: 30px;
}
.time .interval {
  width: 10px;
  display: inline-block;
  line-height: 33px;
}
</style>
