<template>

  <div id="home">
    <div class="mt-4" id="text">
    <h3>{{ currentText }}</h3>
  </div>
  <radial-progress-bar class="m-auto" :strokeWidth='9' :diameter="275" :completed-steps="timeleft" :total-steps="totalSteps" :startColor="startColor" :stopColor="stopColor" :innerStrokeColor="innerStrokeColor">
    <h2>{{ timetext }}</h2>
  </radial-progress-bar>

    <b-btn variant="white" v-if="status != 1" @click="start">
      <font-awesome-icon class="mt-2" id="play" size="2x" :icon="['fas', 'play']" color="white" ></font-awesome-icon>
    </b-btn>
    <b-btn variant="white" v-if="status == 1" @click="pause">
      <font-awesome-icon class="mt-4" id="pause" size="2x" :icon="['fas', 'pause']" color="white"></font-awesome-icon>
    </b-btn>
    <b-btn variant="white" v-if="current.length > 0 || todos.length > 0" @click="finish(true)">
      <font-awesome-icon class="mt-2" id="forward" size="2x" :icon="['fas', 'step-forward']" color="white"></font-awesome-icon>
    </b-btn>
  </div>

</template>

<script>
export default {
  data () {
    return {
      // 0 = 停止
      // 1 = 播放
      // 2 = 暫停
      status: 0,
      timer: 0,
      totalSteps: 5,
      startColor: '#ffffff77',
      stopColor: '#ffffff77',
      innerStrokeColor: '#ffffff00'
    }
  },
  computed: {
    currentText () {
      return this.current.length > 0 ? this.current : this.todos.length > 0 ? '開始囉٩(๑´0`๑)۶' : '增加工作清單吧٩(ˊᗜˋ*)و'
    },
    timetext () {
      const m = Math.floor(this.timeleft / 60)
      const s = Math.floor(this.timeleft % 60)
      return `${m} : ${s}`
    },
    alarm () {
      return this.$store.getters.alarm
    },
    timeleft () {
      return this.$store.getters.timeleft
    },
    current () {
      return this.$store.getters.current
    },
    todos () {
      return this.$store.getters.todos
    }
  },
  methods: {
    start () {
      if (this.status === 2) {
        // 暫停後繼續
        this.status = 1
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft < 0) {
            this.finish(false)
          }
        }, 1000)
      } else {
        // 開始新倒數
        if (this.todos.length > 0) {
          this.$store.commit('start')
          this.status = 1
          this.timer = setInterval(() => {
            this.$store.commit('countdown')
            if (this.timeleft < 0) {
              this.finish(false)
            }
          }, 1000)
        }
      }
    },
    finish (skip) {
      clearInterval(this.timer)
      this.status = 0
      this.$store.commit('finish')

      if (!skip) {
        const audio = new Audio()
        audio.src = './alarms/' + this.alarm
        audio.play()
      }

      if (this.todos.length > 0) {
        this.start()
      } else {
        alert('ʕっ•ᴥ•ʔっ完成了所有事項!!⊂(･ω･*⊂)')
      }
    },
    pause () {
      clearInterval(this.timer)
      this.status = 2
    }
  }
}
</script>
