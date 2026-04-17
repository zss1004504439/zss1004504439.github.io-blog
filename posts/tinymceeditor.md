---
id: gZyu3a
title: TinymceEditor
createdAt: "2021-03-19 15:58:16"
updated: "2026-04-17 11:19:41"
tags: []
tag_ids: []
categories: []
published: true
hideInList: false
feature: ""
isTop: false
---

### 安装
```bash
yarn add tinymce @tinymce/tinymce-vue
```
### public文件下引入皮肤
```
        skin_url: '/tinymce/skins/ui/oxide',
        content_css: `/tinymce/skins/content/default/content.min.css`,
```

```html
<template>
  <div class="editor">
    <TinymceEditor id="editor-tinymce" ref="ZEditor" v-model="myValue" :init="completeSetting" :disabled="disabled" />
  </div>
</template>

<script>
// https://www.cnblogs.com/huihuihero/p/13877589.html
import tinymce from 'tinymce/tinymce'
import TinymceEditor from '@tinymce/tinymce-vue'
// 样式 引入node_modules里的tinymce相关文件文件
// // import 'tinymce/skins/content/default/content.min.css'
// import 'tinymce/skins/ui/oxide/skin.min.css'
// import 'tinymce/skins/ui/oxide/content.min.css'

// 主题
import 'tinymce/themes/silver/theme'
// 图标
import 'tinymce/icons/default/icons'

import 'tinymce/plugins/advlist' // 高级列表
import 'tinymce/plugins/anchor' // 锚点
import 'tinymce/plugins/autolink' // 自动链接
import 'tinymce/plugins/autoresize' // 编辑器高度自适应,注：plugins里引入此插件时，Init里设置的height将失效
import 'tinymce/plugins/autosave' // 自动存稿

import 'tinymce/plugins/charmap' // 特殊字符
import 'tinymce/plugins/code' // 编辑源码
import 'tinymce/plugins/codesample' // 代码示例
import 'tinymce/plugins/colorpicker'
// import 'tinymce/plugins/contextmenu'
import 'tinymce/plugins/directionality' // 文字方向
import 'tinymce/plugins/emoticons' // 表情
import 'tinymce/plugins/fullscreen' // 全屏插件
import 'tinymce/plugins/help'
import 'tinymce/plugins/hr'

import 'tinymce/plugins/image' // 图片插件
import 'tinymce/plugins/imagetools'
import 'tinymce/plugins/importcss' // 引入css
import 'tinymce/plugins/insertdatetime' // 插入日期时间

import 'tinymce/plugins/link' // 超链接
import 'tinymce/plugins/lists' // 列表插件
import 'tinymce/plugins/media' // 插入编辑媒体
import 'tinymce/plugins/noneditable'
import 'tinymce/plugins/nonbreaking' // 插入不间断空格
import 'tinymce/plugins/quickbars' // 快速工具栏

import 'tinymce/plugins/table' // 表格
import 'tinymce/plugins/tabfocus' // 切入切出，按tab键切出编辑器，切入页面其他输入框中
import 'tinymce/plugins/template' // 内容模板
// import 'tinymce/plugins/textcolor' //文字颜色
import 'tinymce/plugins/textpattern' // 快速排版
import 'tinymce/plugins/wordcount' // 字数统计
import 'tinymce/plugins/save' // 保存
import 'tinymce/plugins/searchreplace' // 查找替换

import 'tinymce/plugins/pagebreak' // 插入分页符
import 'tinymce/plugins/paste' // 粘贴插件
import 'tinymce/plugins/preview' // 预览
import 'tinymce/plugins/print' // 打印
import 'tinymce/plugins/toc' // 目录生成器
import 'tinymce/plugins/visualblocks' // 显示元素范围
import 'tinymce/plugins/visualchars' // 显示不可见字符

// import 'tinymce/plugins/importword'
// import 'tinymce/plugins/indent2em'
// import 'tinymce/plugins/formatpainter'

export default {
  name: 'ZEditor',
  components: {
    TinymceEditor
  },
  props: {
    value: {
      type: String,
      default: ''
    },
    setting: {
      type: Object,
      default: () => {}
    },
    disabled: {
      type: Boolean,
      default: false
    },
    plugins: {
      type: [String, Array],
      default:
        'print preview searchreplace autolink directionality visualblocks visualchars fullscreen image link media template code codesample table charmap hr pagebreak nonbreaking anchor insertdatetime advlist lists wordcount textpattern autosave '
    },
    toolbar: {
      type: [String, Array],
      default:
        'fullscreen undo redo restoredraft | cut copy paste pastetext | forecolor backcolor bold italic underline strikethrough link anchor | alignleft aligncenter alignright alignjustify outdent indent | \ styleselect formatselect fontselect fontsizeselect | bullist numlist | blockquote subscript superscript removeformat | \ table image media charmap hr pagebreak insertdatetime print preview | code selectall searchreplace visualblocks | indent2em lineheight formatpainter axupimgs'
    }
  },
  data () {
    return {
      defaultSetting: {
        language_url: '/admin_jzm/tinymce/langs/zh_CN.js',
        language: 'zh_CN',
        // theme: 'silver',
        // theme_url: '/admin_jzm/tinymce/mytheme.js',

        // 这样设置不用 import css文件
        // skin: 'oxide',
        skin_url: 'http://127.0.0.1:8001/admin_jzm/tinymce/skins/ui/oxide',
        content_css: `/tinymce/skins/content/default/content.min.css`,
        min_height: 250,
        max_height: 600,
        selector: 'textarea',
        plugins:
          'print preview searchreplace autolink directionality visualblocks visualchars fullscreen image link media template code codesample table charmap hr pagebreak nonbreaking anchor insertdatetime advlist lists wordcount imagetools textpattern help noneditable autosave indent2em autoresize formatpainter importword quickbars ',

        toolbar: `code undo redo restoredraft | cut copy paste pastetext | forecolor backcolor bold italic underline strikethrough link anchor | alignleft aligncenter alignright alignjustify outdent indent | \
        styleselect formatselect fontselect fontsizeselect | bullist numlist | blockquote subscript superscript removeformat | \
        table image media charmap hr pagebreak insertdatetime print preview | codesample fullscreen | indent2em lineheight formatpainter importword restoredraft`,
        toolbar_sticky: false, // 粘性工具栏
        quickbars_insert_toolbar: '', // quickimage quicktable
        toolbar_drawer: 'sliding',
        quickbars_selection_toolbar:
          'removeformat | bold italic underline strikethrough | fontsizeselect forecolor backcolor',
        branding: false, // 隐藏右下角技术支持
        menubar: true, // 隐藏最上方menu
        statusbar: false, // 隐藏编辑器底部的状态栏
        convert_urls: false,
        toolbar_mode: 'wrap', // sliding & wrap

        // 字号
        fontsize_formats: '12px 14px 16px 18px 24px 36px 48px 56px 72px',
        // 字体
        font_formats: '微软雅黑=Microsoft YaHei,Helvetica Neue,PingFang SC,sans-serif;苹果苹方=PingFang SC,Microsoft YaHei,sans-serif;宋体=simsun,serif;仿宋体=FangSong,serif;黑体=SimHei,sans-serif;Arial=arial,helvetica,sans-serif;Arial Black=arial black,avant garde;Book Antiqua=book antiqua,palatino;Comic Sans MS=comic sans ms,sans-serif;Courier New=courier new,courier;Georgia=georgia,palatino;Helvetica=helvetica;Impact=impact,chicago;Symbol=symbol;Tahoma=tahoma,arial,helvetica,sans-serif;Terminal=terminal,monaco;Times New Roman=times new roman,times;Verdana=verdana,geneva;Webdings=webdings;Wingdings=wingdings,zapf dingbats;知乎配置=BlinkMacSystemFont, Helvetica Neue, PingFang SC, Microsoft YaHei, Source Han Sans SC, Noto Sans CJK SC, WenQuanYi Micro Hei, sans-serif;小米配置=Helvetica Neue,Helvetica,Arial,Microsoft Yahei,Hiragino Sans GB,Heiti SC,WenQuanYi Micro Hei,sans-serif',
        link_list: [{ title: '预置链接1', value: 'http://www.baidu.com' }],
        image_list: [{ title: '预置图片1', value: 'https://www.baidu.com/img/bd_logo1.png' }],
        // content_style: `img{}`,
        // importcss_append: true, // 配合插件 importcss
        // content_security_policy: "script-src *;", //内容安全策略
        // extended_valid_elements: 'script[src]', // 扩展有效元素
        // 为内容模板插件提供预置模板
        templates: [
          {
            title: '模板文档',
            description: '模板介绍',
            content:
              '<div class="mceTmpl"><span class="cdate">CDATE</span>，<span class="mdate">MDATE</span>，模板内容</div>'
          }
        ],
        insertdatetime_formats: ['%Y年%m月%d日', '%H点%M分%S秒', '%Y-%m-%d', '%H:%M:%S'],
        paste_data_images: true, // 配合插件 paste 允许粘贴图像
        // autosave_ask_before_unload: true, // 当关闭或跳转URL时，弹出提示框提醒用户仍未保存变更内容。默认开启提示。
        // images_upload_base_path: '/demo', // 给返回的相对路径指定它所相对的基本路径。
        images_upload_handler: (blobInfo, success, failFun) => {
          console.log('🚀 ~ file: ZEditor2.vue ~ line 136 ~ data ~ blobInfo', blobInfo)
          const img = 'data:image/jpeg;base64,' + blobInfo.base64()
          success(img)
        }
      },
      myValue: this.value
    }
  },
  computed: {
    completeSetting () {
      return Object.assign(this.defaultSetting, this.setting)
    }
  },
  watch: {
    myValue (newValue) {
      this.$emit('input', newValue)
    },
    value (newValue) {
      this.myValue = newValue
    }
  },
  mounted () {
    // tinymce.init({})
  },
  beforeDestroy () {
    tinymce.get('editor-tinymce').destroy()
  }
}
</script>

<style lang="less" scoped>
::v-deep .tox-tinymce {
  border: 1px solid #dcdfe6;
  border-radius: 4px;
}
</style>

```