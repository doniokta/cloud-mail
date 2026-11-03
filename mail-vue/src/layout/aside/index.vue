<template>
  <el-scrollbar class="scroll">
    <div class="aside-wrapper">
      <!-- Mac Window Controls -->
      <div class="mac-header">
        <div class="mac-dots">
          <span class="mac-dot red"></span>
          <span class="mac-dot yellow"></span>
          <span class="mac-dot green"></span>
        </div>
      </div>

      <!-- Brand / Logo Section -->
      <div class="brand-section">
        <img class="logo" :src="clashmateLogo" alt="Clashmate.id Mail" />
        <span class="brand-name">{{ brandTitle }}</span>
      </div>

      <!-- New Mail / Compose Button -->
      <button v-perm="'email:send'" class="compose-btn" type="button" @click="openCompose">
        <Icon icon="ic:round-plus" width="20" height="20" />
        <span>{{ $t('compose') || 'New Mail' }}</span>
      </button>

      <!-- Section: Mail -->
      <div class="nav-section-title">Mail</div>
      <div class="nav-list">
        <div class="nav-item" :class="{ active: route.name === 'email' }" @click="router.push({name: 'email'})">
          <div class="nav-left">
            <Icon icon="solar:inbox-line-duotone" width="20" height="20" />
            <span class="nav-label">{{ $t('inbox') }}</span>
          </div>
          <span class="nav-badge" v-if="emailStore.emailScroll?.total">{{ emailStore.emailScroll.total }}</span>
        </div>

        <div v-perm="'email:send'" class="nav-item" :class="{ active: route.name === 'send' }" @click="router.push({name: 'send'})">
          <div class="nav-left">
            <Icon icon="solar:plain-2-line-duotone" width="20" height="20" />
            <span class="nav-label">{{ $t('sent') }}</span>
          </div>
        </div>

        <div v-perm="'email:send'" class="nav-item" :class="{ active: route.name === 'draft' }" @click="router.push({name: 'draft'})">
          <div class="nav-left">
            <Icon icon="solar:pen-new-square-linear" width="19" height="19" />
            <span class="nav-label">{{ $t('drafts') }}</span>
          </div>
        </div>

        <div class="nav-item" :class="{ active: route.name === 'star' }" @click="router.push({name: 'star'})">
          <div class="nav-left">
            <Icon icon="solar:star-line-duotone" width="20" height="20" />
            <span class="nav-label">{{ $t('starred') }}</span>
          </div>
        </div>

        <div class="nav-item" :class="{ active: route.name === 'setting' }" @click="router.push({name: 'setting'})">
          <div class="nav-left">
            <Icon icon="solar:settings-linear" width="20" height="20" />
            <span class="nav-label">{{ $t('settings') }}</span>
          </div>
        </div>
      </div>

      <!-- Section: Manage (Admin routes) -->
      <template v-if="hasAnyManagePerm">
        <div class="nav-section-title">{{ $t('manage') || 'Manage' }}</div>
        <div class="nav-list">
          <div v-perm="'analysis:query'" class="nav-item" :class="{ active: route.name === 'analysis' }" @click="router.push({name: 'analysis'})">
            <div class="nav-left">
              <Icon icon="solar:chart-2-linear" width="20" height="20" />
              <span class="nav-label">{{ $t('analytics') }}</span>
            </div>
          </div>

          <div v-perm="'user:query'" class="nav-item" :class="{ active: route.name === 'user' }" @click="router.push({name: 'user'})">
            <div class="nav-left">
              <Icon icon="solar:users-group-rounded-linear" width="20" height="20" />
              <span class="nav-label">{{ $t('allUsers') }}</span>
            </div>
          </div>

          <div v-perm="'all-email:query'" class="nav-item" :class="{ active: route.name === 'all-email' }" @click="router.push({name: 'all-email'})">
            <div class="nav-left">
              <Icon icon="solar:letter-linear" width="20" height="20" />
              <span class="nav-label">{{ $t('allMail') }}</span>
            </div>
          </div>

          <div v-perm="'role:query'" class="nav-item" :class="{ active: route.name === 'role' }" @click="router.push({name: 'role'})">
            <div class="nav-left">
              <Icon icon="solar:lock-password-linear" width="20" height="20" />
              <span class="nav-label">{{ $t('permissions') }}</span>
            </div>
          </div>

          <div v-perm="'reg-key:query'" class="nav-item" :class="{ active: route.name === 'reg-key' }" @click="router.push({name: 'reg-key'})">
            <div class="nav-left">
              <Icon icon="solar:key-linear" width="20" height="20" />
              <span class="nav-label">{{ $t('inviteCode') }}</span>
            </div>
          </div>

          <div v-perm="'setting:query'" class="nav-item" :class="{ active: route.name === 'sys-setting' }" @click="router.push({name: 'sys-setting'})">
            <div class="nav-left">
              <Icon icon="solar:shield-check-linear" width="20" height="20" />
              <span class="nav-label">{{ $t('SystemSettings') }}</span>
            </div>
          </div>
        </div>
      </template>
    </div>
  </el-scrollbar>
</template>

<script setup>
import router from "@/router/index.js";
import {computed} from "vue";
import { useRoute } from "vue-router";
import {Icon} from "@iconify/vue";
import {useSettingStore} from "@/store/setting.js";
import {useUiStore} from "@/store/ui.js";
import {useEmailStore} from "@/store/email.js";
import {hasPerm} from "@/perm/perm.js";
import clashmateLogo from "@/assets/logo.png";

const settingStore = useSettingStore();
const uiStore = useUiStore();
const emailStore = useEmailStore();
const route = useRoute();

const brandTitle = computed(() => {
  return settingStore.settings.title || 'Clashmate.id Mail';
});

const hasAnyManagePerm = computed(() => {
  return hasPerm(['all-email:query','user:query','role:query','setting:query','analysis:query','reg-key:query']);
});

function openCompose() {
  uiStore.writerRef.open();
}
</script>

<style lang="scss" scoped>
.scroll {
  height: 100vh;
  background: #ffffff;
  border-right: 1px solid #edf0f4;
}

.aside-wrapper {
  padding: 16px 14px 24px;
  width: 250px;
  box-sizing: border-box;
}

/* Mac window controls */
.mac-header {
  padding: 4px 6px 16px;
}

.mac-dots {
  display: flex;
  align-items: center;
  gap: 8px;
}

.mac-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
  cursor: pointer;
  transition: transform 0.15s ease;

  &:hover {
    transform: scale(1.1);
  }

  &.red {
    background: #ff5f56;
    border: 1px solid #e0443e;
  }
  &.yellow {
    background: #ffbd2e;
    border: 1px solid #dea123;
  }
  &.green {
    background: #27c93f;
    border: 1px solid #1aab29;
  }
}

/* Brand */
.brand-section {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 6px 18px;

  .logo {
    width: 32px;
    height: 32px;
    object-fit: contain;
    border-radius: 6px;
  }

  .brand-name {
    font-size: 16px;
    font-weight: 700;
    color: #0f172a;
    letter-spacing: -0.2px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}

/* Compose Button */
.compose-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  width: 100%;
  height: 44px;
  background: #1d68ff;
  color: #ffffff;
  border: none;
  border-radius: 12px;
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(29, 104, 255, 0.25);
  transition: all 0.2s ease;
  margin-bottom: 22px;

  &:hover {
    background: #1557e0;
    box-shadow: 0 6px 16px rgba(29, 104, 255, 0.35);
    transform: translateY(-1px);
  }

  &:active {
    transform: translateY(0);
  }
}

/* Section titles */
.nav-section-title {
  font-size: 12px;
  font-weight: 600;
  color: #94a3b8;
  text-transform: capitalize;
  padding: 0 10px;
  margin-top: 14px;
  margin-bottom: 8px;
  letter-spacing: 0.3px;
}

/* Navigation items */
.nav-list {
  display: flex;
  flex-direction: column;
  gap: 3px;
}

.nav-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 9px 12px;
  border-radius: 10px;
  color: #64748b;
  cursor: pointer;
  transition: all 0.15s ease;
  user-select: none;
  border: 1px solid transparent;

  .nav-left {
    display: flex;
    align-items: center;
    gap: 12px;

    .nav-label {
      font-size: 14px;
      font-weight: 500;
    }
  }

  .nav-badge {
    font-size: 12px;
    font-weight: 600;
    color: #64748b;
    background: #f1f5f9;
    padding: 2px 7px;
    border-radius: 10px;
  }

  &:hover {
    background: #f8fafc;
    color: #1e293b;
  }

  &.active {
    background: #ffffff;
    border-color: #e2e8f0;
    color: #0f172a;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);

    .nav-left {
      color: #1d68ff;

      .nav-label {
        font-weight: 600;
        color: #0f172a;
      }
    }

    .nav-badge {
      color: #1d68ff;
      background: #eff6ff;
    }
  }
}
</style>
