<template>
  <div>
    <ul>
      <li><a class="refresh" v-html="icons.refresh"></a></li>
      <li><a class="active">发布</a></li>
      <li><a>图片</a></li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>
div
{
  background-color: white;
  border-radius: 5px;
}

ul
{
  display: flex;
  list-style: none;
  padding: 0 15px;
  li{
    a
    {
      display: inline-block;
      padding: 0.5em 1em;
      color: #000;
      text-decoration: none;

      border-bottom: 3px solid #fff;
      &.refresh
      {
        border: none;
        padding: 0.5em;
        transition: color ease-in-out .1s;
        &:hover
        {
          color: #6495ed;
        }
      }
      &.active
      {
        border-bottom: 3px solid #6495ed;
        &:hover
        {
          cursor: default;
        }
      }
      &:hover
      {
        cursor: pointer;
      }
    }
  }
}
</style>

<script>
import feather from 'feather-icons'

const refresh = feather.icons["rotate-cw"].toSvg({width: "18px"})

export default {
  data(){return{
    icons:{
      refresh: refresh
    }
  }}
}
</script>