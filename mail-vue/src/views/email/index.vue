<template>
  <div class="inbox-split-container">
    <div class="inbox-list-col" :class="{ 'is-mobile': !isDesktop }">
      <emailScroll ref="scroll"
                   :cancel-success="cancelStar"
                   :star-success="addStar"
                   :getEmailList="getEmailList"
                   :emailDelete="emailDelete"
                   :star-add="starAdd"
                   :star-cancel="starCancel"
                   :email-read="emailRead"
                   :show-unread="true"
                   :highlight-text="emailStore.searchParams.text"
                   actionLeft="4px"
                   @jump="jumpContent"
      >
      </emailScroll>
    </div>
    <div class="inbox-detail-col" v-if="isDesktop">
      <ContentView :is-embedded="true" />
    </div>
  </div>
</template>

<script setup>
import {useAccountStore} from "@/store/account.js";
import {useEmailStore} from "@/store/email.js";
import {useSettingStore} from "@/store/setting.js";
import emailScroll from "@/components/email-scroll/index.vue";
import ContentView from "@/views/content/index.vue";
import {emailList, emailDelete, emailLatest, emailRead} from "@/request/email.js";
import {starAdd, starCancel} from "@/request/star.js";
import {defineOptions, onMounted, onBeforeUnmount, ref, watch} from "vue";
import {sleep} from "@/utils/time-utils.js";
import router from "@/router/index.js";
import { useRoute } from 'vue-router'

defineOptions({
  name: 'email'
})

const route = useRoute();
const emailStore = useEmailStore();
const accountStore = useAccountStore();
const settingStore = useSettingStore();
const scroll = ref({});
const isDesktop = ref(window.innerWidth >= 1024);

const handleResize = () => {
  isDesktop.value = window.innerWidth >= 1024;
};

onMounted(() => {
  emailStore.emailScroll = scroll;
  window.addEventListener('resize', handleResize);
  latest();
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize);
});

watch(() => accountStore.currentAccountId, () => {
  scroll.value.refreshList();
});

watch(
  () => emailStore.searchParams,
  () => {
    scroll.value.refreshList();
  },
  {deep: true}
);

function jumpContent(email) {
  emailStore.contentData.email = emailStore.toContentEmail(email);
  emailStore.contentData.delType = 'logic';
  emailStore.contentData.showUnread = true;
  emailStore.contentData.showStar = true;
  emailStore.contentData.showReply = true;
  if (!isDesktop.value) {
    router.push({name: 'content', params: {mailbox: 'inbox'}});
  }
}

const existIds = new Set();

async function latest() {
  while (true) {
    let autoRefresh = settingStore.settings.autoRefresh;
    await sleep(autoRefresh > 1 ? autoRefresh * 1000 : 3000);

    if (route.name !== 'email') {
      continue;
    }

    const searchParams = emailStore.searchParams;
    if (searchParams.text || searchParams.startDate || searchParams.endDate) {
      continue;
    }

    const latestId = scroll.value.latestEmail?.emailId;

    if (!scroll.value.firstLoad && autoRefresh > 1) {
      try {
        const accountId = accountStore.currentAccountId;
        const allReceive = scroll.value.latestEmail?.allReceive;
        let list = [];

        if (accountId === scroll.value.latestEmail?.reqAccountId) {
          list = await emailLatest(latestId, accountId, allReceive);
        }

        if (accountId === accountStore.currentAccountId && allReceive === accountStore.currentAccount.allReceive) {
          if (list.length > 0) {
            emailStore.applyFullList(list);

            for (let email of list) {
              email.reqAccountId = accountId;
              email.allReceive = allReceive;

              if (!existIds.has(email.emailId)) {
                existIds.add(email.emailId);
                scroll.value.addItem(email);
                await sleep(50);
              }
            }
          }
        }
      } catch (e) {
        if (e.code === 401 || e.code === 403) {
          settingStore.settings.autoRefresh = 0;
        }
        console.error(e);
      }
    }
  }
}

function addStar(email) {
  emailStore.starScroll?.addItem(email);
}

function cancelStar(email) {
  emailStore.starScroll?.deleteEmail([email.emailId]);
}

function getEmailList(emailId, size) {
  const accountId = accountStore.currentAccountId;
  const allReceive = accountStore.currentAccount.allReceive;
  return emailStore.fetchList(full =>
    emailList(accountId, allReceive, emailId, 0, size, 0, full, emailStore.searchParams)
  ).then(data => {
    data.latestEmail.reqAccountId = accountId;
    data.latestEmail.allReceive = allReceive;
    return data;
  });
}
</script>

<style lang="scss" scoped>
.inbox-split-container {
  display: flex;
  height: 100%;
  width: 100%;
  overflow: hidden;
  background: #f4f6f8;
}

.inbox-list-col {
  width: 380px;
  min-width: 340px;
  max-width: 440px;
  height: 100%;
  border-right: 1px solid #edf0f5;
  background: #f4f6f8;
  flex-shrink: 0;

  &.is-mobile {
    width: 100%;
    max-width: none;
    border-right: none;
  }
}

.inbox-detail-col {
  flex: 1;
  height: 100%;
  background: #ffffff;
  overflow: hidden;
}

.icon {
  cursor: pointer;
}
</style>
