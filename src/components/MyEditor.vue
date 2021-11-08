<script setup>
// import { h } from 'snabbdom'
import { computed, onUnmounted, ref } from 'vue'
// import { Boot, i18nChangeLanguage } from '@wangeditor/editor'
import { Editor, Toolbar, getEditor, removeEditor } from '@wangeditor/editor-for-vue'
import cloneDeep from 'lodash.clonedeep'



// // 测试：多语言
// i18nChangeLanguage('en')

// // 测试：renderElem
// function renderElemFn(elem, children) {
//     const vnode = h('div', {}, children) // type: 'paragraph' 节点，即渲染为 <p> 标签
//     return vnode
// }
// const renderElemConf = {
//     type: 'paragraph', // 节点 type ，重要！！！
//     renderElem: fn,
// }
// Boot.registerRenderElem(renderElemConf)




// 编辑器 id ，全局唯一且不变！！！
const editorId = 'wangeEditor-1'

// 默认内容
// const defaultContent = []
// const getDefaultContent = computed(() => cloneDeep(defaultContent))
const defaultContent = ref([])
const getDefaultContent = computed(() => cloneDeep(defaultContent.value))

const flag = ref(false)

// 模拟 ajax 异步获取内容
setTimeout(() => {
    flag.value = true
    defaultContent.value =  [
        {
            type: "paragraph",
            children: [{ text: "ajax 异步获取的内容" }],
        },
    ]
}, 1000)

// 编辑器配置
const editorConfig = {
    placeholder: '请输入内容...',
    MENU_CONF: {
        insertImage: {
            checkImage(src) {
                console.log('image src', src)
                if (src.indexOf("http") !== 0) {
                    return "图片网址必须以 http/https 开头";
                }
                return true;
            },
        },
    }
}

// // 工具栏配置
// const toolbarConfig = {
//     toolbarKeys: [],
//     excludeKeys: [],
// }

// 编辑器回调函数
const handleCreated = (editor) => {
    console.log("created", editor);
}
const handleChange = (editor) => {
    console.log("change:", editor.children);
}
const handleDestroyed = (editor) => {
    console.log('destroyed', editor)
}
const handleFocus = (editor) => {
    console.log('focus', editor)
}
const handleBlur = (editor) => {
    console.log('blur', editor)
}
const customAlert = (info, type) => {
    alert(`【自定义提示】${type} - ${info}`)
}
const customPaste = (editor, event, callback) => {
    console.log('ClipboardEvent 粘贴事件对象', event)

    // 自定义插入内容
    editor.insertText('xxx')

    // 返回值（注意，vue 事件的返回值，不能用 return）
    callback(false) // 返回 false ，阻止默认粘贴行为
    // callback(true) // 返回 true ，继续默认的粘贴行为
}

// 及时销毁编辑器
onUnmounted(() => {
    const editor = getEditor(editorId)
    if (editor == null) return

    // 销毁，并移除 editor
    editor.destroy()
    removeEditor(editorId)
})

const getHtml = () => {
    const editor = getEditor(editorId)
    if (editor == null) return

    console.log(editor.getHtml())
}
</script>

<template>
    <div>
        <button @click="getHtml">获取 html</button>
    </div>
    <div style="border: 1px solid #ccc" v-if="flag">
        <!-- 工具栏 -->
        <Toolbar
            :editorId="editorId"
            :defaultConfig="toolbarConfig"
            style="border-bottom: 1px solid #ccc"
        />
        <!-- 编辑器 -->
        <Editor
            :editorId="editorId"
            :defaultConfig="editorConfig"
            :defaultContent="getDefaultContent"
            @onChange="handleChange"
            @onCreated="handleCreated"
            @onDestroyed="handleDestroyed"
            @onFocus="handleFocus"
            @onBlur="handleBlur"
            @customAlert="customAlert"
            @customPaste="customPaste"
            style="height: 500px"
        />
    </div>
</template>

<style src="@wangeditor/editor/dist/css/style.css"></style>
