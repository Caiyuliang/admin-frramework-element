<template>
  <div id="app">
    <transition name="fade" mode="out-in">
				<keep-alive v-if="$route.query.keepAlive">
					<router-view></router-view>
				</keep-alive>
				<router-view v-if="!$route.query.keepAlive">
					<router-view></router-view>
				</router-view>
      <!-- <router-view></router-view> -->
    </transition>
  </div>
</template>

<script>
export default {
	name: 'app',
	data () {
    return {
      permissions: this.$store.getters.info.role,
      options: {
        role: this.$store.getters.info.role,
        permissions: this.$store.getters.info.permissions,
      }
    };
  },
	mounted() {
	//	console.log(this.$route)
	},
	created() { 
    console.log(this.options,this.permissions)
  },
	watch: {
    permissions (newQuestion, oldQuestion) {
      switch (newQuestion) {
        case 'superAdmin':
          this.options = {
            role: 'superAdmin',
            permissions: '超级管理员'
          }
          break
        case 'admin':
          this.options = {
            role: 'admin',
            permissions: '管理员'
          }
          break
        case 'ordinary':
          this.options = {
            role: 'ordinary',
            permissions: '普通用户'
          }
      }
      this.$store.dispatch('setRole', this.options)

      //  刷新 tabnav 权限管理测试需要
      this.$store.dispatch('removeOtherTab', {tabItem: {
          title: 'pageControl',
          path: '/pagePermissions'
        }, router: this.$router})
    }
  }
}
 
</script>

<style lang="scss">
body {
	margin: 0px;
	padding: 0px;
	font-family: Microsoft YaHei, Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB,  SimSun, sans-serif;
	font-size: 14px;
	-webkit-font-smoothing: antialiased;
}

#app {
	position: absolute;
	top: 0px;
	bottom: 0px;
	width: 100%;
}
a{
  color: #56a9ff;
}
.fade-enter-active,
.fade-leave-active {
	transition: all .2s ease;
}

.fade-enter,
.fade-leave-active {
	opacity: 0;
}
</style>
