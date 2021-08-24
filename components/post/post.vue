<template>
  <div class="post">
    <div class="header">
      <img :src="post.avatar" :alt="post.name">
      <p class="name">{{post.name}}</p>
      <p class="datetime"><time :datetime="post.time">{{this.timestring}}</time></p>
    </div>
    <div class="content" v-html="html"></div>
    <div class="function">
      <p>{{timeago}}</p>
      <ul>
        <li><a class="like" :liked="liked" @click="like" v-html="icons.thumbsUp"></a></li>
        <li><a v-html="icons.comment"></a></li>
        <li><a v-html="icons.repost"></a></li>
      </ul>
    </div>
  </div>
</template>

<script>
import marked from 'marked'
import { format } from 'fecha'
import { format as timeago } from 'timeago.js'
import feather from 'feather-icons'

const thumbsUp = feather.icons["thumbs-up"].toSvg()
const comment = feather.icons["message-square"].toSvg()
const repost = feather.icons["corner-up-right"].toSvg()

export default {
  props:{
    post:{
      name: String,
      time: Date,
      avatar: String,
      content: String
    }
  },
  data(){return{
    icons:{
      thumbsUp: thumbsUp,
      comment: comment,
      repost: repost
    },
    liked: false
  }},
  computed:{
    html: function()
    {
      return marked(this.post.content)
    },
    timestring: function()
    {
      return format(this.post.time, 'YYYY-MM-DD HH:mm:ss Z')
    },
    timeago: function()
    {
      return timeago(this.post.time, "zh_CN")
    },
  },
  methods:{
    like: function()
    {
      if (this.liked === true)
        this.liked = false
      else
        this.liked = true
    }
  },
  mounted(){
    marked.setOptions(
      {
        gfm: true,
        breaks: true
      }
    )
  }
}
</script>

<style lang="scss">
// 内联样式需要去掉 scoped 才有效

.post
{
  &:not(:last-child)
  {
    margin-bottom: 15px;
  }
  padding: 15px;
  background-color: white;
  border-radius: 5px;

  .header
  {
    min-height: 50px;
    margin-bottom: 15px;

    &:after
    {
      clear: both;
    }

    img
    {
      float: left;
      width: 50px;
      height: 50px;
      border: none;
      border-radius: 50%;
      transition: all ease-in-out .25s;
      &:hover
      {
        transform: scale(1.25);
      }
    }

    p
    {
      margin-left: 63px;
      margin-bottom: 0;
      &.datetime
      {
        font-size: 0.8em;
        color: #555;
      }
    }
  }
  .content
  {
    margin-bottom: 15px;

    code
    {
      color: saturate( #6495ed , 50);
      background-color: #eaeaea;
      border-radius: 6px;
      padding: .3em .4em;
    }

    pre
    {
      padding: 15px;
      background-color: #eaeaea;
      border-radius: 5px;
      code
      {
        color: #000;
      }
    }
  }
  .function
  {
    display: flex;
    p
    {
      margin: 0;
    }

    ul
    {
      margin-left: auto;
      margin-bottom: 0;
      list-style: none;
      padding: 0;
      display: flex;
      li
      {
        margin: 0 1em;
        a
        {
          color: #000;
          transition: color ease-in-out .1s;
          &:hover
          {
            cursor: pointer;
            color: #6495ed;
          }
        }
      }
    }

    .like[liked="true"]
    {
      color: #6495ed;
      // https://css-tricks.com/adding-shadows-to-svg-icons-with-css-and-svg-filters/
      svg
      {
        filter: drop-shadow(0 0 1px #6495ed);
      }
    }
  }
}
</style>