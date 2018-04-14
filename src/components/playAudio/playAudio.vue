<template>
  <div class="audio">
    <div class="progress-wrapper">
      <span class="time time-l">{{format(currentTime)}}</span>
      <!--播放进度条-->
      <div class="progress-bar-wrapper">
        <progress-bar :percent="percent" @percentChange="onProgressBarChange"></progress-bar>
      </div>
      <span class="time time-r">{{format(duration)}}</span>
    </div>
    <audio :src="src"
           ref="audio"
           id="audio"
           @play="ready"
           @error="error"
           @canplay="canplay"
           @timeupdate="updateTime"
           @ended="ended"
           preload="auto"
    >
    </audio>
  </div>
</template>
<script type="text/ecmascript-6">
  import progressBar from '@/components/progressBar/progressBar'

  export default {
    name: 'play-audio',
    props: {
      src: {
        type: String
      }
    },
    data() {
      return {
        currentTime: 0,
        playing: false,
        duration: 0
      }
    },
    components: {
      progressBar
    },
    mounted() {
      this.audioAutoPlay()
    },
    computed: {
      percent() {
        return this.currentTime / this.duration
      }
    },
    watch: {
      playing(newPlaying) {
        const audio = this.$refs.audio
        this.$nextTick(() => {
          newPlaying ? audio.play() : audio.pause()
        })
      }
    },
    methods: {
      audioAutoPlay() {
        this.$nextTick(() => {
          this.playing = true
          document.addEventListener("WeixinJSBridgeReady", function () {
            this.playing = true
          }, false);
          document.addEventListener("YixinJSBridgeReady", function () {
            this.playing = true
          }, false);
        })
      },
      onProgressBarChange(percent) {
        const currentTime = this.duration * percent
        this.$refs.audio.currentTime = currentTime
        if (!this.playing) {
          this.togglePlaying()
        }
      },
      ready() {
        console.log('ready')
        this.playing = true
      },
      error() {
        console.log('error')
      },
      canplay(ev) {
        this.duration = ev.target.duration
      },
      updateTime(ev) {
        this.currentTime = ev.target.currentTime
      },
      togglePlaying() {
        this.playing = !this.playing
      },
      ended() {
        console.log('ended')
        this.playing = false
      },
      // 格式化时间
      format(interval) {
        interval = interval | 0
        const minute = interval / 60 | 0
        const second = this._pad(interval % 60)
        return `${minute}:${second}`
      },
      _pad(num, n = 2) {
        let len = num.toString().length
        while (len < n) {
          num = '0' + num
          len++
        }
        return num
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/variable.styl"
  .audio
    .progress-wrapper
      display: flex
      align-items: center
      width: 80%
      margin: 0px auto
      padding: 10px 0
      .time
        color: $color-text
        font-size: $font-size-small
        flex: 0 0 30px
        line-height: 30px
        width: 30px
        &.time-l
          text-align: left
        &.time-r
          text-align: right
      .progress-bar-wrapper
        flex: 1
</style>
