<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand href="/">空间</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <!-- <b-nav-item href="#">Link</b-nav-item> -->
        </b-navbar-nav>
        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <!-- 分隔 -->

    <b-container class="page" fluid>
      <b-row>
        <b-col md="5" class="mx-auto">
          <div class="center">
            <editor @publish="publish" />
            <headbar />
            <transition-group name="posts">
              <!-- key 不对，过渡效果也不对 -->
              <post v-for="post in posts" :key="post.time.valueOf()" :post="post" />
            </transition-group>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<style scoped>
.page
{
  /* background-color: #22272e; */
  background-image: url("/microblog/bg/b3.png");
  background-size: cover;
  background-attachment: fixed;
  min-height: 100vh;
}
.center
{
  margin: 0 auto;
  max-width: 720px;
}

.posts-enter-active,
.posts-leave-active
{
  transition: all 1s;
}

.posts-enter,
.posts-leave-to
{
  opacity: 0;
}
</style>

<script>
import editor from '~/components/post/editor.vue'
import headbar from '~/components/post/headbar.vue'
import post from '~/components/post/post.vue'
import feather from 'feather-icons'

const user = feather.icons["user"].toSvg()

const about = `## 简易微博

- 布局
- 组件
- 简易编辑器
- markdown 支持 \`gfm\`
- 伪发布
- 伪图片上传
- 图片预览和删除
- 过渡效果
- 点赞

由 [literal](https://github.com/sinsong) 制作
`;

export default {
  components: {editor, headbar, post},
  data(){return{
    posts: [
      // {name: "", avatar: "", time: "", content: ""}

      {name: "莉特雅", avatar: "/microblog/avatar/literal.png", time: new Date("2021-08-24 17:34:00+0800"), content: about},
      {name: "莉特雅", avatar: "/microblog/avatar/literal.png", time: new Date("2021-08-17 16:46:14+0800"), content: "您好！欢迎使用！"},
      {name: "lo娘莉特雅", avatar: "/microblog/avatar/a1.jpg", time: new Date("2021-08-17 16:57:15+0800"), content: "好耶！"},
    ]
  }},
  methods:{
    // 伪发布
    publish: function(content){
      this.posts.unshift({
        name: "游客",
        avatar: "data:image/svg+xml," + user,
        time: new Date(),
        content: content
      })
    }
  }
}
</script>