<template>
  <div class="ebook" ref="ebook">
    <ebook-bookmark></ebook-bookmark>
    <ebook-header></ebook-header>
    <ebook-title></ebook-title>
    <router-view></router-view>
    <ebook-menu></ebook-menu>
    <ebook-footer></ebook-footer>
  </div>
</template>
<script>
import EbookTitle from '@/components/ebook/EbookTitle'
import EbookMenu from '@/components/ebook/EbookMenu'
import { ebookMixin } from '@/utils/mixin'
import { getReadTime, saveReadTime } from '../../utils/localStorage'
import EbookHeader from '../../components/ebook/EbookHeader'
import EbookFooter from '../../components/ebook/EbookFooter'
import EbookBookmark from '../../components/ebook/EbookBookmark'
export default {
  name: 'Ebook',
  mixins: [ebookMixin],
  components: {
    EbookBookmark,
    EbookFooter,
    EbookHeader,
    EbookTitle,
    EbookMenu
  },
  watch: {
    offsetY (v) {
      if (this.isPaginating !== null && this.isPaginating === false && !this.menuVisible) {
        if (v === 0) {
          this.restore()
        } else if (v > 0) {
          this.move(v)
        }
      }
    }
  },
  methods: {
    restore () {
      this.$refs.ebook.style.top = 0
      this.$refs.ebook.style.transition = 'all .2s liner'
      setTimeout(() => {
        this.$refs.ebook.style.transition = ''
      }, 200)
    },
    move (v) {
      this.$refs.ebook.style.top = v + 'px'
    },
    startLoopReadTime () {
      let readTime = getReadTime(this.fileName)
      if (!readTime) {
        readTime = 0
      }
      this.task = setInterval(() => {
        readTime++
        if (readTime % 30 === 0) {
          saveReadTime(this.fileName, readTime)
        }
      }, 1000)
    }
  },
  created () {
    this.setGlobalTheme()
  },
  mounted () {
    this.startLoopReadTime()
  },
  beforeDestroy () {
    if (this.task) {
      clearInterval(this.task)
    }
  }
}
</script>
<style lang='scss' scoped>
@import "../../assets/styles/global";

.ebook {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
}
</style>
