<template>
  <p>
    wangEditor for vue3
    <button @click="toggle">显示/隐藏</button>
  </p>
  <my-editor v-if="show"></my-editor>
  <!-- <my-editor-extend v-if="show"></my-editor-extend> -->
</template>

<script>
import MyEditor from './components/MyEditor.vue'
// import MyEditorExtend from './components/MyEditorExtend.vue'

export default {
  name: 'App',
  components: {
    MyEditor,
    // MyEditorExtend,
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
