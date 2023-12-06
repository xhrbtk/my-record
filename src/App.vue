<script setup>
import { reactive, ref } from 'vue'
let mediaRecorder = null
let audioData = []
let recordList = reactive([])
let nameText = ref('')
const startRecord = () => {
    console.log('开始录音')
    navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {
            // 创建媒体记录
            mediaRecorder = new MediaRecorder(stream);
            // 开始录制
            mediaRecorder.start()
          // 处理音频数据
            mediaRecorder.addEventListener('dataavailable', ev => {
            // 把数据块添加到数组
            audioData.push(ev.data)
            let blob = new Blob(audioData, { 'type' : 'audio/ogg; codecs=opus' })
            const url = URL.createObjectURL(blob)
            recordList.push({ name: new Date(), url: url })
            console.log('recordList', recordList)
        })
    }).catch(err => {
          alert('无法获取麦克风权限！错误信息：' + err);
    })
}
const stopRecord = () => {
    console.log('停止录音')
    //  录音停止
    mediaRecorder.stop()
}

const playRecord = () => {
    console.log('播放录音')
    if (audioData === null) {
      alert('没有录音播放')
      return 
    }
    

}
</script>

<template>
  <input class="nameInput" v-model="nameText" placeholder="请输入">
  <div class="wrapper">
    <button class="startBtn btn" @click="startRecord">开始录音</button>
    <button class="stopBtn btn" @click="stopRecord">停止录音</button>
  </div>
  <ul class="audioList">
      <li  v-for="item in recordList" :key="item.id">
        <span class="name">{{ item.name }}</span>
        <audio controls :src="item.url" ></audio>
      </li>
  </ul>

</template>

<style lang="scss" scoped>
.wrapper{
  // width: 300px;
  display: flex;
  justify-content: space-between;
  width: 300px;
  margin-top: 10px;
  .btn{
    border: none;
    color: #fff;
    margin-right: 10px;
    &:focus {
      outline: none;
    }
  }
  .playBtn{
    background: #1989fa;
  }
  .startBtn{
    background: #1989fa;
  }
  .stopBtn{
    background: #ee0a24;
  }
}
.audioList{
  margin-top: 20px;
  min-height: 100px;
  >ul{
    list-style: none;
  }
  >li{
    list-style: none;
    display: flex;
    align-items: center;
    margin-top: 10px;
    .name{
      font-size: 16px; 
    }
  }
}
.nameInput{
  display: inline-block;
  width: 300px;
  height: 40px;
  border: 1px solid #eff2f5;
  border-radius: 10px;
}
</style>
