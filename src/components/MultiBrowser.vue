<template>
  <div class="dropdown">
    <!--<input  v-model="searchQuery" @input="filterOptions">-->
    <ul>
      <li v-for="(option, index) in filteredOptions"
          :key="index" @click="selectOption(option)">
        <img class="icons" v-if=" option.name==='百度'" src="../assets/images/baidu-bg.svg" alt="">
        <img class="icons" v-else-if=" option.name==='谷歌'" src="../assets/images/google-circle-fill.svg" alt="">
        <img class="icons" v-else src="../assets/images/bing.svg" alt="">
        {{ option.name }}
      </li>
    </ul>
  </div>
</template>


<script lang="ts">
import Vue, {ref} from 'vue';

export default {
  setup(props, {emit}) {
    const filteredOptions = [
      {name: '百度', url: 'https://www.baidu.com/s?wd='},
      {name: '谷歌', url: 'https://www.Google.com/search?q='},
      {name: '必应', url: 'https://www.cn.bing.com/search?q='}
    ];
    const options = ref();
    const icons = ref();//获取名字
    const selectOption = (option) => {
      // console.log(option);
      options.value = option.url;
      icons.value = option.name;//获取名字
      console.log(options.value);
      console.log(icons.value);
      emit('switchBrowser', options.value, icons.value);//绑定切换浏览器的地址与名字
    };


    return {
      filteredOptions,
      selectOption,
      options,
      icons
    };

  },


};
</script>

<style lang="scss" scoped>
.dropdownMenu > .browserList {
  position: absolute;
  background: #ffffff;
  top: 38px;
  z-index: 2;
  text-align: left;
  width: 170px;
  border-radius: 4px;
}
.dropdown{
  ul{
    position: relative;

    &::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      border: 3px solid rgba(168, 239, 255, 1);
      transition: all .5s;
      animation: clippath 3s infinite linear;
    }
  }

  @keyframes clippath {
    0%,
    100% {
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


.dropdown {
  ul {
    li {
      padding: 10px;
      animation: slide-down 0.3s ease-in;
      &:hover{
        color: red;
        font-size:120%;
      }
    }
  }
}


</style>