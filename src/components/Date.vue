<template>
  <div>
    {{ date }}
  </div>
</template>

<script lang="ts" setup>
import formatter from './formatterDate.vue';
import {ref, onMounted} from 'vue';

const date = ref('');
onMounted(() => {
  setInterval(() => {
    getDate();
  }, 1000);
});

const getDate = function () {
  date.value = formatter(new Date());
};


</script>

<style lang="scss" scoped>
div {
  width: 12%;
  position: relative;
  text-align: center;
  font-size: 16px;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border-radius: 20px;
    border: 2px solid rgb(168, 239, 255, 1);
    transition: all .5s;
    animation: clippath 3s infinite linear;
  }

  @keyframes clippath {
    0%, 100% {
      clip-path: inset(0 0 95% 0);
    }
    25% {
      clip-path: inset(0 95% 0 0);
    }
    50% {
      clip-path: inset(95% 0 0 0);
    }
    75% {
      clip-path: inset(0 0 0 95%);
    }
  }
}

@media(max-width: 500px) {
  div {
    width: 172px
  }
}

.date {


  position: fixed;
  bottom: 20px;
  right: 20px;
  color: white;
  z-index: 3;
  text-align: center;
  white-space: pre-wrap;
  background: -webkit-linear-gradient(left, #0f0, #00f) 0 0 no-repeat; /*设置线性渐变*/
  -webkit-background-size: 80px; /*设置背景大小*/
  -webkit-background-clip: text; /*背景被裁剪到文字*/
  -webkit-text-fill-color: rgba(255, 255, 255, 0.3); /*设置文字的填充颜色*/
  -webkit-animation: shine 3s infinite;
}

@-webkit-keyframes shine { /*创建动画*/
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 100% 100%;
  }
}
</style>