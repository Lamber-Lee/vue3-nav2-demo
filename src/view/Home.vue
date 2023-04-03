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
          {{ name }}
        </div>
        <MultiBrowser @switchBrowser="switchBrowser" v-if="showOptions" class="browserList"></MultiBrowser>
      </div>
      <button @click="search">
        <img class="icons" src="../assets/images/search.svg" alt="">
      </button>
    </div>
    <main class="globalMain">
      <ul class="siteList">

        <li class="sites" v-for="(site,index) in sites" :key="index">
          <img :src=site.faviconUrl alt="icon">
          <a :href="site.url" target="_blank">{{ site.name }}</a>
          <span class="deleteIcon" @click="deleteSite(index)">X</span>
        </li>
        <li class="addButton">
          <div class="text" @click="addButton">
            <img src="../assets/images/add.svg" alt="">
            新增网址
          </div>
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
    MultiBrowser,

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
    const x = localStorage.getItem('x'); //读取x数据


    const xObject = JSON.parse(x);// json字符串转化为json对象
    const sites = ref();

    const deleteSite = (e) => { //删除不想要的网址
      sites.value.splice(e, 1);
      localStorage.removeItem('x');//从内存里移除
      alert('删除成功');
      localStorage.setItem('x', JSON.stringify(sites.value));//删除后 返回新的存储x值
    };
    const initSites = [
      {name: 'baidu', url: 'https://baidu.com', faviconUrl: 'https://baidu.com/favicon.ico'},
      {name: 'bilibili', url: 'https://bilibili.com', faviconUrl: 'https://www.bilibili.com/favicon.ico'},
      {name: 'taobao', url: 'https://taobao.com', faviconUrl: 'https://www.taobao.com/favicon.ico'},
      {name: 'youku', url: 'https://youku.com', faviconUrl: 'https://www.youku.com/favicon.ico'},
      {name: 'qq', url: 'https://y.qq.com', faviconUrl: 'https://y.qq.com/favicon.ico'},
    ];


    if (xObject) {
      sites.value = xObject;
    } else {
      sites.value = initSites;
    }
    const search = () => {
      window.location.href = msg.value + inputValue.value;//选择浏览器后 进行搜索

    };
    const addButton = () => {
      let addUrl = window.prompt('请问你要添加的网址是啥？');
      if (addUrl.indexOf('http') !== 0) {
        addUrl = 'https://' + addUrl;
        // && !/^https?:\/\//i.test(addUrl)
      }
      const url = new URL(addUrl) //先创建 URL 对象
      let faviconUrl = `${url.protocol}//${url.hostname}/favicon.ico`;//再构建出一个favicon的URL链接
      //url.protocol 指的是URL链接的协议，例如http或者https等,url.hostname 指的是URL链接的主机名或者域名
      console.log(faviconUrl);
      const img = new Image();
      img.src = faviconUrl;
      img.onload = function () {
        // 图标加载完成后，将其存储在Vue组件中
        // 然后使用它来显示书签的图标
      };
      img.onerror = function () {
        // 如果使用常见的路径无法找到图标，尝试使用其他路径.png
        faviconUrl = `${url.protocol}//${url.hostname}/favicon.png`;
        img.src = faviconUrl;
        // 继续使用onload事件处理程序和错误处理程序处理加载和错误
      };
      console.log(addUrl);
      const simplifyUrl = (addUrl) => {
        return addUrl.replace('https://', '')
            .replace('http://', '')
            .replace('www.', '')
            .replace(/\/.*/, '')// 删除 / 开头的内容
            .replace('.com', '');// 删除 .com 后缀
      };
      if (x) { //判断x是否存在
        if (x.includes(simplifyUrl(addUrl).toLowerCase())) {
          alert('请勿重复添加标签名');
        } else {
          sites.value.push({name: simplifyUrl(addUrl).toLowerCase(), url: addUrl, faviconUrl: faviconUrl});
          alert('添加成功');
        }
      }

    };
    console.log(sites.value);
    const string = JSON.stringify(sites.value);
    localStorage.setItem('x', string); //存储json字符串数据到本地
    const showBrowser = () => {
      showOptions.value = !showOptions.value; //值取非 点击展示浏览器列表

    };
    let showOptions = ref(false);
    const msg = ref();
    const name = ref();//获取名字
    const switchBrowser = (msg2, name2) => {//切换浏览器
      console.log(msg2);
      console.log(name2);
      msg.value = msg2;
      name.value = name2;
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