<template>
  <div class="comment-parent" v-if="commentList">
    <div
      class="comment-wrapper"
      v-for="parentItem in commentList"
      :commentList="parentItem"
      :key="parentItem.comment_id"
    >
      <div class="comment-item">
        <div class="avatar" V-if="parentItem.userinfo">
          <img v-if="parentItem.userinfo.user_img" v-lazy="parentItem.userinfo.user_img" alt />
          <img v-else src="~assets/logo.png" alt />
        </div>
        <div class="main-wrapper">
          <div class="title" v-if="parentItem.userinfo">
            <span v-if="parentItem.userinfo.name">{{parentItem.userinfo.name}}</span>
            <span v-else>此用户没有设置昵称（官方）</span>
            <span>{{parentItem.comment_date}}</span>
          </div>
          <div class="content">{{parentItem.comment_content}}<span @click="replyWho(parentItem)" class="reply">喷</span></div>
        </div>
      </div>
      <commentChild class="comment-child" :commentChildList="parentItem.child" @replyWhoChild="replyWho"></commentChild>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import commentChild from "./commentChild";
export default {
  name: "comment",
  props: ["commentList"],
  data() {
    return {};
  },
  methods: {
    //点击发送回复对象信息
    replyWho(item){
      this.$emit("replyWho",item)
    },  
  },
  components: {
    commentChild,
  },
};
</script>

<style lang="less" scoped>
.comment-parent {
  .comment-wrapper {
    border-bottom: 1px solid #e7e7e7;
    .comment-item {
      display: flex;
      padding: 12px 0;
      .avatar {
        margin-right: 15px;
        img {
          width: 35px;
          height: 35px;
          border-radius: 50%;
        }
      }
      .main-wrapper {
        flex: 1;
        .title {
          font-size: 13px;
          color: #555;
          display: flex;
          justify-content: space-between;
          margin-bottom: 5px;
        }
        .content {
          word-break: break-all; //换行
          font-size: 13px;
          position: relative;
          .reply{
            position: absolute;
            right: 8px;
            font-size: 14px;
            color:#fb7299;
          }
        }
      }
    }
    .comment-child {
      padding-left: 40px;
    }
  }
}
</style>
