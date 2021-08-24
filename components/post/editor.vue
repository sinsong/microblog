<template>
  <div class="editor-panel">
    <div class="editor">
      <textarea v-model="content" id="editpost" ref="editpost" :rows="rows" placeholder="要说什么呢？"></textarea>
      <input type="file" @change="fileinputchange" name="file" ref="editfile" id="editfile" accept="image/*">
    </div>

    <div class="images">
      <imagepreview v-for="file in previews" :key="file.name" :file="file" @deletefile="deletefile" />
    </div>

    <div class="toolbox">
      <button class="image"  v-html="icons.image"  @click="uploadImage"></button>
      <button class="bold"   v-html="icons.bold"   @click="insertFormat('**','**')"></button>
      <button class="italic" v-html="icons.italic" @click="insertFormat('_','_')"></button>
      <button class="quote"  v-html="icons.quote"  @click="insertFormat('#', '')"></button>
      <button class="code"   v-html="icons.code"   @click="insertFormat('`','`')"></button>
      <button class="at"     v-html="icons.at"     @click="insertFormat('@', '')"></button>
      <!-- 发布按钮 -->
      <button class="publish" @click="$emit('publish', content), content = ''" :disabled="content.length == 0">发布</button>
    </div>
  </div>
</template>

<script>
import feather from 'feather-icons'

import imagepreview from '~/components/post/imagepreview.vue'

const image = feather.icons["image"].toSvg()
const bold = feather.icons["bold"].toSvg()
const italic = feather.icons["italic"].toSvg()
const at = feather.icons["at-sign"].toSvg()
const code = feather.icons["code"].toSvg()
const quote = feather.icons["message-square"].toSvg()

export default {
  components: { imagepreview },
  data(){return{
    content: "",
    rows: 3,
    icons:{
      image, bold, italic, at, code, quote
    },
    files: [],
    previews: []
  }},
  watch:{
    content: function(val)
    {
      var rowNum = val.split("\n").length
      if(rowNum < 3){this.rows = 3; return}
      if(rowNum > 10){this.rows = 10; return}
      this.rows = rowNum;
    }
  },
  methods:{
    // 获取选择
    selection: function()
    {
      if(process.client)
      {
        var editor = this.$refs.editpost
        return {
          begin: editor.selectionStart,
          end: editor.selectionEnd,
        }
      }
    },
    // 设置选择
    setSelection: function(begin, end)
    {
      if(process.client)
      {
        var editor = this.$refs.editpost
        // https://stackoverflow.com/a/66868511
        this.$nextTick(()=>{
          editor.setSelectionRange(begin, end)
        })
      }
    },
    // 插入 markdown 相关字符
    insertFormat: function(inBeg, inEnd)
    {
      const { begin, end } = this.selection()
      var oldcontent = this.content
      var content = oldcontent.substring(0, begin) + (begin != end ? inBeg + oldcontent.substring(begin, end ) + inEnd : inBeg + inEnd) + oldcontent.substring(end)
      this.setContent(content)
      this.focusEditor()
      this.setSelection(begin != end ? begin + inBeg.length : begin + inBeg.length,
                        (begin != end ? end : begin) + inBeg.length)
    },
    // 设置内容
    setContent: function(content){
      this.content = content
    },
    // 获取焦点
    focusEditor: function(){
      if(process.client)
      {
        this.$refs.editpost.focus()
      }
    },
    // 激活文件输入表单项目
    uploadImage: function(){
      if(process.client)
      {
        this.$refs.editfile.click()
      }
    },
    // 输入文件
    fileinputchange: function(){
      const fileinput = this.$refs.editfile
      for (const file of fileinput.files)
      {
        for(const efile of this.files)
        {
          if (efile.name === file.name)
          {
            // alert("重复上传图片")
            return
          }
        }

        this.files.push(file)
        this.previews.push({
          name: file.name,
          previewURL: URL.createObjectURL(file)
        })
      }
    },
    // 删除文件
    deletefile: function(name)
    {      
      this.files.forEach(function(file, index, array){
        if ( file.name === name )
          array.splice(index, 1)
      })

      this.previews.forEach(function(file, index, array){
        if ( file.name === name )
          array.splice(index, 1)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.editor-panel
{
  margin: 15px 0;
  padding: 15px;
  border-radius: 5px;
  background-color: white;
}

.editor
{
  font-size: 16px;
  textarea
  {
    width: 100%;
    resize: none;

    margin-bottom: 15px;
    padding: 0.5em;

    border: 1px solid #aaa;
    border-radius: 5px;

    box-shadow: none;
    &:focus
    {
      box-shadow: none;
      appearance: none;
      outline: #6495ed solid 2px;
    }
  }
  input[type="file"]
  {
    display: none;
  }
}

.images
{
  overflow-x: auto;
  white-space: nowrap;
}

.toolbox
{
  display: flex;

  button
  {
    border: none;
    background: transparent;
    margin: 0 0.1em;

    transition: color ease-in-out .1s;
    &:hover
    {
      color: #6495ed;
    }
  }

  button.publish
  {
    margin-left: auto;

    padding: 0.25em 1.5em;

    color: #fff;
    background-color: #6495ed;
    border:none;
    border-radius: 2px;

    &:hover
    {
      background-color: lighten(#6495ed, 5);
    }

    &[disabled]
    {
      background-color: grayscale(#6495ed)
    }
  }
}
</style>