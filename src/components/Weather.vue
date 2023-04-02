<template>
  <div class="weather">
    <div id="he-plugin-simple"></div>
  </div>
</template>

<script>
import  {defineComponent,onMounted} from 'vue';

window.WIDGET = {
  "CONFIG": {
    "modules": "10234",
    "background": "4",
    // "backgroundColor": "#94515b",
    "tmpColor": "#FFFFFF",
    "tmpSize": "16",
    "cityColor": "#FFFFFF",
    "citySize": "16",
    "aqiColor": "#FFFFFF",
    "aqiSize": "16",
    "weatherIconSize": "24",
    "alertIconSize": "18",
    "padding": "10px 10px 10px 10px",
    "shadow": "1",
    "language": "auto",
    "borderRadius": "5",
    "fixed": "false",
    "vertical": "top",
    "horizontal": "left",
    "key": "9d5221dc8ace4810bb4e5ac1d4f47cef"
  }
};
export default defineComponent({
  setup() {
    onMounted(() => { //监听组件挂载完成的事件
      weather()
    });
    const weather = () => { //加载天气预报的 JavaScript 脚本
      const s = document.createElement("script");
      s.type = "text/javascript";
      s.src =
          "https://widget.qweather.net/simple/static/js/he-simple-common.js?v=2.0";
      document.body.appendChild(s);
    };

    return {};
  }
});

</script>

<style lang="scss" scoped>

.weather {
  position: fixed;
  top: 80px;
  z-index: 3;
}
</style>