<template lang="pug">
  .jobs(:style="{ backgroundImage: `url(img/${color[0]}-strip.png)`, marginTop: `${group.margin}px`, paddingTop: `${group.padding}px`, paddingBottom: `${group.padding}px` }")
    .job(v-for="(job, i) in jobs" :key="`j-${i - 1}`"  :class="{ active: job.enabled }" :style="{ paddingTop: `${group.padding}px`, marginTop: `${group.padding}px`, marginBottom: `${group.padding}px` }")
      a(:href="job.url" target="_blank" :style="{ color: `#${color[1]}` }") {{ job.name }}
</template>

<script>
export default {
  name: 'mental-health-jobs',
  props: ['color', 'jobs', 'group'],
  data() {
    return {
    }
  },
  computed: {

  },
  methods: {
    followLink () {

    }
  },
  created () {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
  $margin: 6px;
  $left-gap: 10px;
  $right-gap: 10px;

  .jobs {
    display: block;
    width: 100%;
    min-height: 10px;
    background-repeat: repeat-y;
    background-position: top left;
    padding: $margin 0;
    margin: $margin 0;

    .job {
      background: rgba(255, 255, 255, 0.5);

      &.active { background: white; }
      border-radius: .5em;
      margin: 5px 20px;
      padding: 5px;
      font-size: 8pt;
      font-weight: 700;

      a {
        text-decoration: none;
      }
    }
  }
</style>
