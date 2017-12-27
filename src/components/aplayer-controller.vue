<template>
  <div class="aplayer-controller">
    <v-progress
            :loadProgress="loadProgress"
            :playProgress="playProgress"
            :theme="theme"
            @setprogress="val => $emit('setprogress', val)"
            @dragbegin="$emit('dragbegin')"
            @dragend="val => $emit('dragend', val)"
            @dragging="val => $emit('dragging', val)"
    ></v-progress>
    <div class="aplayer-time">
      <span class="aplayer-time-inner">
        - <span class="aplayer-ptime">{{secondToTime(stat.playedTime)}}</span> / <span
              class="aplayer-dtime">{{secondToTime(stat.duration)}}</span>
      </span>
      <volume
              :volume="volume"
              :theme="theme"
              :muted="muted"
              @togglemute="$emit('togglemute')"
              @setvolume="v => $emit('setvolume', v)"
      ></volume>
      <icon-button class="aplayer-icon-mode" :icon="mode" @click.native="$emit('nextmode')"></icon-button>
      <icon-button class="aplayer-icon-menu" icon="menu" @click.native="$emit('togglelist')"></icon-button>
    </div>
  </div>
</template>

<script>
  import IconButton from './aplayer-iconbutton.vue'
  import VProgress from './aplayer-controller-progress.vue'
  import Volume from './aplayer-controller-volume.vue'

  export default {
    components: {
      IconButton,
      VProgress,
      Volume,
    },
    props: ['mode', 'stat', 'theme', 'volume', 'muted'],
    computed: {
      loadProgress() {
        if (this.stat.duration === 0) return 0
        return this.stat.loadedTime / this.stat.duration
      },
      playProgress() {
        if (this.stat.duration === 0) return 0
        return this.stat.playedTime / this.stat.duration
      },
    },
    methods: {
      secondToTime(second) {
        if (isNaN(second)) {
          return '00:00';
        }
        const pad0 = (num) => {
          return num < 10 ? '0' + num : '' + num;
        };

        const min = Math.trunc(second / 60);
        const sec = Math.trunc(second - min * 60);
        const hours = Math.trunc(min / 60);
        const minAdjust = Math.trunc((second / 60) - (60 * Math.trunc((second / 60) / 60)));
        return second >= 3600 ? pad0(hours) + ':' + pad0(minAdjust) + ':' + pad0(sec) : pad0(min) + ':' + pad0(sec);
      },
      jump() {

      },
    },
  }
</script>