<template>
  <div class="home">
    <play-audio :src="audioSrc"></play-audio>
  </div>
</template>
<script type="text/ecmascript-6">
  import playAudio from '@/components/playAudio/playAudio'

  export default {
    name: 'home',
    data() {
      return {
        audioSrc: 'http://thirdparty.gtimg.com/C100001Qu4I30eVFYb.m4a?fromtag=38'
      }
    },
    components: {
      playAudio
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
