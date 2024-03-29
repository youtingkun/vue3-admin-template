<!-- 导航栏 -->
<template>
  <div class="navbar">
    <Hamburger
      id="hamburger-container"
      :is-active="sidebar.opened"
      class="hamburger-container"
      @toggle-click="toggleSideBar"
    />
    <BreadCrumb id="breadcrumb-container" class="breadcrumb-container" />
    <div class="right-menu">
      <el-dropdown class="language-container hover-effect right-menu-item" trigger="click">
        <div class="language-wrapper">
          <span class="name">{{ $t('common_lang') }}</span>
          <i class="el-icon-caret-bottom" />
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item
              v-for="lang in state.langs"
              @click="switchLanguage(lang.key)"
              :key="lang.key"
            >
              <div class="language-item">
                {{ lang.name }}
              </div>
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
      <template v-if="device !== 'mobile'">
        <Screenfull class="right-menu-item hover-effect" v-if="showScreenfull" />
        <ThemeSwitch class="right-menu-item hover-effect" v-if="showThemeSwitch" />
      </template>
      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="require('@/assets/layout/avatar.gif')" class="user-avatar" alt="" />
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <router-link to="/">
              <el-dropdown-item> 首页 </el-dropdown-item>
            </router-link>
            <a target="_blank" href="#">
              <el-dropdown-item> 文档 </el-dropdown-item>
            </a>
            <a target="_blank" href="#">
              <el-dropdown-item> Github </el-dropdown-item>
            </a>
            <el-dropdown-item divided @click="logout">
              <span style="display: block"> 退出登录 </span>
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>

<script lang="ts">
import BreadCrumb from '../bread-crumb/index.vue'
import Hamburger from '../hamburger/index.vue'
import ThemeSwitch from '@/components/theme-switch/index.vue'
import Screenfull from '@/components/screenfull/index.vue'
import { computed, reactive, toRefs, defineComponent } from 'vue'
import { useStore } from '@/store'
import { AppActionTypes } from '@/store/modules/app/action-types'
import { UserActionTypes } from '@/store/modules/user/action-types'
import { useRouter } from 'vue-router'
import i18n, { setLanguage, getLanguage } from '@/language/index'
import { useI18n } from 'vue-i18n'
import { ElMessageBox, ElMessage } from 'element-plus'
export default defineComponent({
  name: 'NavigationBar',
  components: {
    BreadCrumb,
    Hamburger,
    ThemeSwitch,
    Screenfull
  },
  setup() {
    const { t } = useI18n()
    const store = useStore()
    const router = useRouter()
    const sidebar = computed(() => {
      return store.state.app.sidebar
    })
    const device = computed(() => {
      return store.state.app.device.toString()
    })
    const showThemeSwitch = computed(() => {
      return store.state.settings.showThemeSwitch
    })
    const showScreenfull = computed(() => {
      return store.state.settings.showScreenfull
    })
    const state: any = reactive({
      toggleSideBar: () => {
        store.dispatch(AppActionTypes.ACTION_TOGGLE_SIDEBAR, false)
      },
      logout: () => {
        store.dispatch(UserActionTypes.ACTION_LOGIN_OUT, undefined)
        router.push('/login').catch((err) => {
          console.warn(err)
        })
      },
      langs: [
        {
          key: 'en_US',
          name: 'English'
        },
        {
          key: 'zh_CN',
          name: '中文'
        }
      ]
    })
    const switchLanguage = (lang: string) => {
      setLanguage(lang)
      ElMessageBox.alert(`${t('common_lang')}`, '', {
        confirmButtonText: t('common_confirm')
      })
    }
    return {
      state,
      switchLanguage,
      sidebar,
      device,
      showThemeSwitch,
      showScreenfull,
      ...toRefs(state)
    }
  }
})
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    padding: 0 15px;
    cursor: pointer;
    transition: background 0.3s;
    -webkit-tap-highlight-color: transparent;

    &:hover {
      background: rgba(0, 0, 0, 0.025);
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 50px;

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;

      &.hover-effect {
        cursor: pointer;
        transition: background 0.3s;

        &:hover {
          background: rgba(0, 0, 0, 0.025);
        }
      }
    }
    .language-container {
      .language-wrapper {
        cursor: pointer;
      }
    }

    .avatar-container {
      .avatar-wrapper {
        margin-top: 5px;
        margin-right: 16px;
        margin-left: 16px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
