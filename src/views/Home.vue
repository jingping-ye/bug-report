<template>
  <div class="test-com">
    <a-spin :spinning="isLoading" :delay="delayTime">
      <div class="spin-content">
        可以点击‘切换’按钮，延迟显示 loading 效果。当 spinning 状态在 `delay` 时间内结束，则不显示 loading 状态。
      </div>
    </a-spin>
    <a-button type="primary" @click="startRequest">发起请求</a-button>
    <a-button type="primary" @click="clearTimer">清除定时器</a-button>
  </div>
</template>
<script>
let timer = null;
export default {
  name: "TestCom",
  data() {
    return {
      //  状态
      isLoading: false,
      delayTime: 3000,
    };
  },
  methods: {
    /**
     * 开始请求
     */
    startRequest() {
      this.isLoading = true;
      this.mockRequest()
        .then((res) => {
          console.log("res", res);
        })
        .catch((err) => {
          console.log("err", err);
        })
        .finally(() => {
          this.isLoading = false;
          this.setTimer();
        });
    },
    /***
     * 设置定时器
     */
    setTimer() {
      this.clearTimer();
      timer = setTimeout(() => {
        this.startRequest();
      }, 3000);
    },
    /***
     * 清除定时器
     */
    clearTimer() {
      clearTimeout(timer);
      timer = null;
    },
    /***
     * 模拟请求
     */
    mockRequest() {
      // 1000模拟正常请求返回时间; 8000模拟超长请求返回时间
      let delayTimeArr = [1000, 8000];
      let randomIndex = Math.random() > 0.5 ? 1 : 0;
      let delayTime = delayTimeArr[randomIndex];
      console.log("delayTime", delayTime);
      return new Promise((reslove) => {
        setTimeout(() => {
          reslove(1231);
        }, delayTime);
      });
    },
  },
};
</script>
