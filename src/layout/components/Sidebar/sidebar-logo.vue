<!-- 侧边栏logo(需要跟随侧边栏折叠) -->

<template>
  <div class="sidebar-logo-container" :class="{ collapse: !collapse }">
    <transition name="sidebarLogoFade">
      <router-link v-if="!collapse" key="collapse" class="sidebar-logo-link" to="/">
        <img src="@/assets/layout/logo.png" class="sidebar-logo" alt="" />
      </router-link>
      <router-link v-else key="expand" class="sidebar-logo-link" to="/">
        <img src="@/assets/layout/logo-text.png" class="sidebar-logo-text" alt="" />
      </router-link>
    </transition>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'SidebarLogo',
  props: {
    collapse: {
      type: Boolean,
      default: true
    }
  }
})
</script>

<style lang="scss" scoped>
.sidebarLogoFade-enter-active,
.sidebarLogoFade-leave-active {
  transition: opacity 1.5s;
}

.sidebarLogoFade-enter-from,
.sidebarLogoFade-leave-to {
  opacity: 0;
}

.sidebar-logo-container {
  position: relative;
  width: 100%;
  height: 84px;
  line-height: 84px;
  background: #0c202b;
  text-align: center;
  overflow: hidden;

  .sidebar-logo {
    display: none;
  }

  & .sidebar-logo-link {
    height: 100%;
    width: 100%;

    & .sidebar-logo-text {
      width: 205px;
      height: 84px;
      vertical-align: middle;
    }

    & .sidebar-title {
      display: inline-block;
      margin: 0;
      color: #fff;
      font-weight: 600;
      line-height: 50px;
      font-size: 16px;
      font-family: Avenir, Helvetica Neue, Arial, Helvetica, sans-serif;
      vertical-align: middle;
    }
  }

  &.collapse {
    .sidebar-logo {
      width: 32px;
      height: 32px;
      vertical-align: middle;
      margin-right: 0;
      display: inline-block;
    }
    .sidebar-logo-text {
      display: none;
    }
  }
}
</style>
