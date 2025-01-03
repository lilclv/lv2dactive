<template>
  <div>
    <canvas
      style="position: absolute; bottom: 0; right: 0"
      ref="liveCanvas"
    ></canvas>
  </div>
  <div style="display: flex; justify-content: space-between">
    <button
      :disabled="buttonStatus"
      @click="motionOption(5, '主人不要摸头，人要没了')"
      style="background: red"
    >
      触摸
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(2, 'bro文松手！')"
      style="background: red"
    >
      拍头
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(7, 'bro文我的防晒油就靠你了哟')"
      style="background: red"
    >
      防晒油
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(10, '接下本小姐的飞吻吧bro文')"
      style="background: red"
    >
      飞吻
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(11, 'bro文你的小黄鸡被我玩弄了哟，战栗吧')"
      style="background: red"
    >
      攻击黄鸡
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(12, '别乱摸！')"
      style="background: red"
    >
      摸腿
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(13, '你。。。。。')"
      style="background: red"
    >
      袭击
    </button>
    <button
      :disabled="buttonStatus"
      @click="motionOption(14, 'bro文sama，好久不见呢。。。。')"
      style="background: red"
    >
      问候
    </button>
  </div>
  <div class="dialog-box" v-if="showDialog">
    <div class="dialog-content">
      <p>{{ dialogMessage }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import * as PIXI from "pixi.js"
import { Live2DModel } from "pixi-live2d-display/cubism4";
const dialogMessage = ref('');
const buttonStatus = ref(false)
const showDialog = ref(false);
window.PIXI = PIXI
let app
let model
let messageTimer
const liveCanvas = ref(null)
const motionOption = function (type, msg = '欢迎bro文主人') {
  model.update(200000)
  setTimeout(() => {
    model.motion('', type)
    showMessage(msg)
  }, 200)
}
const showMessage = (msg = "", timeout = 6000) => {
  if (messageTimer) {
    clearTimeout(messageTimer)
    messageTimer = null
  } else {
    showDialog.value = true
  }
  dialogMessage.value = msg
  messageTimer = setTimeout(() => {
    showDialog.value = false
    messageTimer = null
  }, timeout)
}

onMounted(async () => {
  app = new PIXI.Application({
    view: liveCanvas.value || undefined,
    autoStart: true,
    toResize: window,
    backgroundAlpha: 0
  })
  model = await (await Live2DModel.from('../../public/tianlangxing_3/tianlangxing_3.model3.json'))
  app.stage.addChild(model);
  model.scale.set(0.1)
});
onBeforeUnmount(() => {
  model?.destroy();
  app?.destroy()
})
</script>

<style scoped>
.dialog-box {
  position: fixed;
  top: 30%;
  left: 25%;
  transform: translate(-30%, -20%);
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  padding: 20px;
  width: 300px;
  z-index: 1000;
}

.dialog-content {
  position: relative;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
  font-size: 20px;
  color: #999;
}

.close-button:hover {
  color: #333;
}

p {
  margin: 0;
  font-size: 16px;
  color: #333;
}
</style>
