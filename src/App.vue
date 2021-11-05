<template>
  <p>
    wangEditor for vue3
    <button @click="toggle">显示/隐藏</button>
  </p>
  <my-editor v-if="show"></my-editor>
</template>

<script>
import MyEditor from './components/MyEditor.vue'

export default {
  name: 'App',
  components: {
    MyEditor,
  },
  data() {
    return {
      show: true
    }
  },
  methods: {
    toggle() {
      this.show = !this.show
    }
  }
}
</script>

<style>
</style>
