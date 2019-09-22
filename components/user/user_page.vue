<template>
  <div class="user">
    <g-header />
    <div class="banner">
      <img src="@/assets/img/user_banner.png" alt="banner">
    </div>
    <div class="user-info">
      <div class="user-info-center">
        <avatar :src="userInfo.avatar" size="100px" class="avatar" />
        <h1 class="username">
          {{ userInfo.name }}
          <!-- {{ userInfo }} -->
        </h1>
        <p class="profile">
          {{ userInfo.introduction || '暂无' }}
        </p>
        <div class="fl jc">
          <div class="fl fdc ac follow-block">
            <span class="follow-number">{{ userInfo.follows || 0 }}</span>
            <span class="follow-title">关注</span>
          </div>
          <div class="fl fdc ac follow-block">
            <span class="follow-number">{{ userInfo.fans || 0 }}</span>
            <span class="follow-title">粉丝</span>
          </div>
        </div>
        <followBtn :id="$route.params.id" class="follow" />
      </div>
    </div>
    <div class="article">
      <userPageNav />
      <slot name="list" />
    </div>
  </div>
</template>

<script>
import { mapState, mapActions, mapGetters } from 'vuex'
import userPageNav from './user_page_nav'
import avatar from '@/components/avatar/index'
import followBtn from '@/components/follow_btn'
export default {
  components: {
    avatar,
    followBtn,
    userPageNav
  },
  computed: {
    ...mapState({
      userInfo: state => state.user.userInfo
    }),
    ...mapGetters(['isMe', 'currentUserInfo'])
  },

  watch: {
    currentUserInfo() {
      this.refreshUser({ id: this.$route.params.id })
    }
  },
  created() {
    if (!this.$route.params.id) this.$router.go(-1)
  },
  mounted() {
    if (this.currentUserInfo.id) this.refreshUser({ id: this.$route.params.id })
  },
  methods: {
    ...mapActions('user', [
      'refreshUser',
      'followOrUnfollowUser'
    ])
  }

}
</script>

<style lang="less" scoped>
.user {
  .minHeight();
}
.banner {
  height: 290px;
  background-color: #C8D7FF;
  img {
    height: 100%;
    display: block;
    margin: 0 auto;
  }
}

.user-info {
  width: 100%;
  height: 220px;
  background-color: #fff;
  box-sizing: border-box;
  &-center {
    width: 766px;
    height: 100%;
    margin: 0 auto;
    padding-top: 1px;
    text-align: center;
    position: relative;
  }
  .avatar {
    border: 4px solid #fff;
    display: inline-block;
    margin-top: -52px;
    background: #fff;
  }
  .username {
    font-size:24px;
    font-weight: bold;
    color: #000;
    line-height:33px;
    padding: 0;
    margin: 10px 0 0;
  }
  .profile {
    font-size:16px;
    font-weight:400;
    color: #000;
    line-height:22px;
    padding: 0;
    margin: 10px 0 20px;
  }
  .follow-block {
    margin: 0 34px;
  }
  .follow-number {
    font-size:24px;
    font-weight:bold;
    color:rgba(84,45,224,1);
    line-height:28px;
    padding: 0;
    margin: 0 0 8px;
  }
  .follow-title {
    font-size:14px;
    font-weight:400;
    color:rgba(0,0,0,1);
    line-height:20px;
    padding: 0;
    margin: 0;
  }
  .follow {
    position: absolute;
    right: 0;
    bottom: 20px;
  }
}

.article {
  width: 766px;
  height: 100%;
  margin: 0 auto;
}
</style>