<script setup lang="ts">
import {computed, onMounted, ref} from 'vue'
import {useMessage} from 'naive-ui'

const message = useMessage()

const playLine = [
  {'name': '2ys', 'url': 'https://gj.fenxiangb.com/player/analysis.php?v=', 'mobile': 0},
  {'name': '虾米', 'url': 'https://jx.xmflv.com/?url=', 'mobile': 1},
  {'name': 'B站1', 'url': 'https://jx.jsonplayer.com/player/?url=', 'mobile': 1},
  {'name': '爱豆', 'url': 'https://jx.aidouer.net/?url=', 'mobile': 1},
  {'name': 'BL', 'url': 'https://vip.bljiex.com/?v=', 'mobile': 0},
  {'name': '冰豆', 'url': 'https://bd.jx.cn/?url=', 'mobile': 0},
  {'name': 'H8', 'url': 'https://www.h8jx.com/jiexi.php?url=', 'mobile': 0},
  {'name': 'JY', 'url': 'https://jx.playerjy.com/?url=', 'mobile': 0},
  {'name': 'M3U8', 'url': 'https://jx.m3u8.tv/jiexi/?url=', 'mobile': 0},
  {'name': 'PM', 'url': 'https://www.playm3u8.cn/jiexi.php?url=', 'mobile': 0},
  {'name': '七哥', 'url': 'https://jx.nnxv.cn/tv.php?url=', 'mobile': 0},
  {'name': '听乐', 'url': 'https://jx.dj6u.com/?url=', 'mobile': 1},
  {'name': '维多', 'url': 'https://jx.ivito.cn/?url=', 'mobile': 0},
  {'name': 'YT', 'url': 'https://jx.yangtu.top/?url=', 'mobile': 0},
  {'name': '夜幕', 'url': 'https://www.yemu.xyz/?url=', 'mobile': 0},
  {'name': '云析', 'url': 'https://jx.yparse.com/index.php?url=', 'mobile': 0},
  {'name': '17云', 'url': 'https://www.1717yun.com/jx/ty.php?url=', 'mobile': 0},
  {'name': '180', 'url': 'https://jx.000180.top/jx/?url=', 'mobile': 0},
  {'name': '4K', 'url': 'https://jx.4kdv.com/?url=', 'mobile': 1}
]

const iframeUrl = computed(() => {
  return vipAddress.value + videoAddress.value
})

const vipAddress = ref(playLine[0].url)
const videoAddress = ref('')

function inputHande(v: string) {
  if (v.indexOf('http') === -1) {
    message.error('请输入正确的视频链接')
    return
  }
  message.success('线路切换成功，请稍后...')
  localStorage.setItem('videoAddress', v)
}

function vipAddressChange(v: string) {
  message.success('地址更新成功，正在加载视频...')
  localStorage.setItem('vipAddress', v)
}

onMounted(() => {
  const address = localStorage.getItem('videoAddress')
  const vip = localStorage.getItem('vipAddress')
  if (address) {
    videoAddress.value = address
  }
  if (vip) {
    vipAddress.value = vip
  }
})
</script>

<template>
  <div class="common-layout">
    <div>
      <div>
        <iframe
            id="playLine"
            :src="iframeUrl"
            width="100%"
            title="YouTube video player"
            allowfullscreen
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        ></iframe>
      </div>
      <div>
        <div>
          <n-form>
            <n-form-item label="播放线路">
              <n-select v-model:value="vipAddress" placeholder="请选择播放线路"
                        :options="playLine"
                        label-field="name"
                        value-field="url"
                        @update:value="vipAddressChange">
              </n-select>
            </n-form-item>
            <n-form-item label="视频链接">
              <n-input type="textarea" v-model:value="videoAddress" rows="4" @input="inputHande"
                       placeholder="请输入视频原链接"/>
            </n-form-item>
          </n-form>
        </div>
        <div class="main-center">
          <n-space justify="center">
            <n-button text tag="a" href="https://www.iqiyi.com" target="_blank">爱奇艺
            </n-button>
            <n-button text tag="a" href="https://v.qq.com" target="_blank">腾讯视频
            </n-button>
            <n-button text tag="a" href="https://www.youku.com" target="_blank">优酷</n-button>
          </n-space>
        </div>
      </div>
    </div>
    <div class="main-center">
      <n-text tag="mark">请注意部分接口存在恶意广告，请斟酌使用；仅供测试</n-text>
    </div>
  </div>
</template>

<style scoped>
@media screen and (max-width: 768px) {
  .common-layout {
    width: 100%;
    height: 100%;
  }

  #playLine {
    width: 100%;
    height: 250px;
  }
}

@media screen and (min-width: 768px) {
  .common-layout {
    width: 768px;
    height: 100%;
    margin: 0 auto;
    padding: 20px 0;
  }

  #playLine {
    width: 100%;
    height: 450px;
  }
}

.main-center {
  text-align: center;
  padding: 10px 0;
}
</style>
