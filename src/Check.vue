<template>
  <div id="check">
    <div class="top">
      <div id="map" style="height: 100%"></div>
    </div>
    <div class="bottom">
      <div
        class="check-btn"
        :style="{ backgroundColor: inCircle ? 'green' : 'red' }"
        @click="onClick"
      >
        <div class="check-btn-inner">
          <Clock />
          <div>点击签到</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, ref, toRaw } from "vue";
import Clock from "./Clock.vue";
import useMap from "./useMap";

export default defineComponent({
  name: "Check",
  components: { Clock },
  setup() {
    const { inCircle, getLocation, currentPos, currentDistance } = useMap(
      "map"
    );
    const onClick = () => {
      getLocation();
      alert(
        JSON.stringify(
          toRaw({
            inCircle: inCircle.value,
            currentPos: currentPos.value,
            currentDistance: currentDistance.value,
          })
        )
      );
    };
    return {
      inCircle,
      onClick,
    };
  },
});
</script>

<style lang="css" scoped>
#check {
  height: 100%;
  display: flex;
  flex-direction: column;
  text-align: center;
}
#check .top {
  height: 50%;
}
#check .bottom {
  flex-grow: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
#check .bottom .check-btn {
  width: 160px;
  height: 160px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid transparent;
  border-radius: 50%;
  cursor: pointer;
}
#check .bottom .check-btn .check-btn-inner {
  width: 140px;
  height: 140px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 1px solid transparent;
  border-radius: 50%;
  background-color: #fff;
}
</style>