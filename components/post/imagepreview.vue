<template>
  <div class="imagepreview" :style="{ backgroundImage: 'url(' + file.previewURL + ')' }">
    <div class="close" v-html="icons.x" @click="$emit('deletefile', file.name)"></div>
  </div>
</template>

<script>
import feather from 'feather-icons'

const x = feather.icons["x"].toSvg()

export default {
  props:{
    file:
    {
      name: String,
      previewURL: String
    }
  },
  data(){return{
    icons:{
      x: x
    }
  }}
}
</script>

<style lang="scss" scoped>
.imagepreview
{
  display: inline-block;
  position: relative;
  width:  100px;
  height: 100px;
  background-size: cover;

  .close
  {
    position: absolute;
    top: 0;
    right: 0;
    cursor: pointer;

    color: #000;
    &:hover
    {
      color: #6495ed;
    }
    
  }
}
</style>