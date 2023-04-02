<template>
  <header class="globalHeader">
    <div class="ls-logo"></div>
    <div class="searchForm">
      <input name="wd" type="text" v-model="inputValue">
      <div class="dropdownMenu">
        <div class="searchBarBtn" @click.stop="showBrowser">
          <img class="icons" v-if=" name==='百度'" src="../assets/images/baidu-bg.svg" alt="">
          <img class="icons" v-else-if=" name==='谷歌'" src="../assets/images/google-circle-fill.svg" alt="">
          <img class="icons" v-else src="../assets/images/bing.svg" alt="">
          {{ name }}</div>
        <MultiBrowser @switchBrowser="switchBrowser" v-if="showOptions" class="browserList"></MultiBrowser>
      </div>
      <button @click="search">
        <img class="icons" src="../assets/images/search.svg" alt="">
      </button>
    </div>
    <main class="globalMain">
      <ul class="siteList">
        <li class="sites" v-for="(site,index) in sites" :key="index">
          <a :href="site.url" target="_blank">{{ site.name }}</a>
          <span class="deleteIcon" @click="deleteSite(index)">X</span>
        </li>
        <li class="addButton">
          <div class="text" @click="addButton">新增网址</div>
        </li>
      </ul>
    </main>
  </header>
</template>

<script lang="ts">
import {onMounted, ref} from 'vue';
import MultiBrowser from '../components/MultiBrowser.vue';

export default {
  components: {
    MultiBrowser
  },
  setup() {
    onMounted(() => {
      document.addEventListener('click', (e) => {
        if (e.target.className !== 'dropdownMenu') {
          showOptions.value = false;
        }
      });
    });

    const inputValue = ref();
    const x = localStorage.getItem('x');
    const xObject = JSON.parse(x);
    const sites = ref();

    const deleteSite = (e) => {
      sites.value.splice(e, 1);
      localStorage.removeItem('x');
      alert('删除成功');
      localStorage.setItem('x', JSON.stringify(sites.value));
    };
    const initSites = [
      {name: 'baidu', url: 'https://baidu.com'},
      {name: 'bilibili', url: 'https://bilibili.com'},
    ];
    if (xObject) {
      sites.value = xObject;
    } else {
      sites.value = initSites;
    }
    const search = () => {
      window.location.href = msg.value + inputValue.value;

    };
    const addButton = () => {
      let addUrl = window.prompt('请问你要添加的网址是啥？');
      if (addUrl.indexOf('http') !== 0) {
        addUrl = 'https://' + addUrl;
        // && !/^https?:\/\//i.test(addUrl)
      }
      console.log(addUrl);

      const simplifyUrl = (addUrl) => {
        return addUrl.replace('https://', '')
            .replace('http://', '')
            .replace('www.', '')
            .replace(/\/.*/, '')// 删除 / 开头的内容
            .replace('.com', '');// 删除 .com 后缀
      };
      if (x.includes(simplifyUrl(addUrl).toLowerCase())) {
        alert('请勿重复添加标签名');
      } else {
        sites.value.push({name: simplifyUrl(addUrl).toLowerCase(), url: addUrl});
        alert('添加成功');
      }
      console.log(sites.value);
      window.onbeforeunload = () => {
        const string = JSON.stringify(sites.value);
        localStorage.setItem('x', string);
      };
    };
    const showBrowser = () => {
      showOptions.value = !showOptions.value; //值取非 点击展示浏览器列表

    };
    let showOptions = ref(false);
    const msg = ref();
    const name = ref();//获取名字
    const switchBrowser = (msg2,name2) => {//切换浏览器
      console.log(msg2);
      console.log(name2)
      msg.value = msg2;
      name.value = name2
    };
    return {
      search,
      sites,
      inputValue,
      addButton,
      deleteSite,
      showBrowser,
      showOptions,
      switchBrowser,
      msg,
      name
    };

  }
};
</script>

<style lang="scss" scoped>
.searchForm {

  position: relative;
}

.dropdownMenu {
  position: absolute;
}

</style>