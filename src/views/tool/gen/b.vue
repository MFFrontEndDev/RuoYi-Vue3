<template>
<div class="app-container">
  <el-button icon="Refresh" @click="resetQuery">跳转测试</el-button>

  <el-popover
      placement="top"
      width="300"
      trigger="hover"
      v-if="showCommitInfo"
    >
      <template #reference>
        <el-button @mouseover="fetchCommit">鼠标悬停显示最新 Commit 信息</el-button>
      </template>
      <div>
        <p>{{ commit.message }}</p>
        <el-button type="primary" link @click="goToGitHub">查看详情</el-button>
      </div>
    </el-popover>
</div>

</template>

<script setup name="operationDetails">
import { nextTick, ref, onMounted, toRaw,reactive } from 'vue'
import ElementPlus from 'element-plus';

const { proxy } = getCurrentInstance();

const showCommitInfo = ref(true);
const mockCommitData = [
  {
    sha: "5e1d3e2c0b7b0f8b8c4b5e5d3e2c0b7b0f8b8c4b",
    commit: {
      author: {
        name: "John Doe",
        email: "john.doe@example.com",
        date: "2024-06-08T12:34:56Z"
      },
      committer: {
        name: "John Doe",
        email: "john.doe@example.com",
        date: "2024-06-08T12:34:56Z"
      },
      message: "Fix issue with user authentication",
      tree: {
        sha: "abcd1234efgh5678ijkl9012mnop3456qrst7890",
        url: "https://api.github.com/repos/{owner}/{repo}/git/trees/abcd1234efgh5678ijkl9012mnop3456qrst7890"
      },
      url: "https://api.github.com/repos/{owner}/{repo}/git/commits/5e1d3e2c0b7b0f8b8c4b5e5d3e2c0b7b0f8b8c4b",
      comment_count: 0,
      verification: {
        verified: false,
        reason: "unsigned",
        signature: null,
        payload: null
      }
    },
    url: "https://api.github.com/repos/{owner}/{repo}/commits/5e1d3e2c0b7b0f8b8c4b5e5d3e2c0b7b0f8b8c4b",
    html_url: "https://github.com/{owner}/{repo}/commit/5e1d3e2c0b7b0f8b8c4b5e5d3e2c0b7b0f8b8c4b",
    comments_url: "https://api.github.com/repos/{owner}/{repo}/commits/5e1d3e2c0b7b0f8b8c4b5e5d3e2c0b7b0f8b8c4b/comments",
    author: {
      login: "johndoe",
      id: 1234567,
      node_id: "MDQ6VXNlcjEyMzQ1Njc=",
      avatar_url: "https://avatars.githubusercontent.com/u/1234567?v=4",
      gravatar_id: "",
      url: "https://api.github.com/users/johndoe",
      html_url: "https://github.com/johndoe",
      followers_url: "https://api.github.com/users/johndoe/followers",
      following_url: "https://api.github.com/users/johndoe/following{/other_user}",
      gists_url: "https://api.github.com/users/johndoe/gists{/gist_id}",
      starred_url: "https://api.github.com/users/johndoe/starred{/owner}{/repo}",
      subscriptions_url: "https://api.github.com/users/johndoe/subscriptions",
      organizations_url: "https://api.github.com/users/johndoe/orgs",
      repos_url: "https://api.github.com/users/johndoe/repos",
      events_url: "https://api.github.com/users/johndoe/events{/privacy}",
      received_events_url: "https://api.github.com/users/johndoe/received_events",
      type: "User",
      site_admin: false
    },
    committer: {
      login: "johndoe",
      id: 1234567,
      node_id: "MDQ6VXNlcjEyMzQ1Njc=",
      avatar_url: "https://avatars.githubusercontent.com/u/1234567?v=4",
      gravatar_id: "",
      url: "https://api.github.com/users/johndoe",
      html_url: "https://github.com/johndoe",
      followers_url: "https://api.github.com/users/johndoe/followers",
      following_url: "https://api.github.com/users/johndoe/following{/other_user}",
      gists_url: "https://api.github.com/users/johndoe/gists{/gist_id}",
      starred_url: "https://api.github.com/users/johndoe/starred{/owner}{/repo}",
      subscriptions_url: "https://api.github.com/users/johndoe/subscriptions",
      organizations_url: "https://api.github.com/users/johndoe/orgs",
      repos_url: "https://api.github.com/users/johndoe/repos",
      events_url: "https://api.github.com/users/johndoe/events{/privacy}",
      received_events_url: "https://api.github.com/users/johndoe/received_events",
      type: "User",
      site_admin: false
    },
    parents: [
      {
        sha: "abcd1234efgh5678ijkl9012mnop3456qrst7890",
        url: "https://api.github.com/repos/{owner}/{repo}/commits/abcd1234efgh5678ijkl9012mnop3456qrst7890",
        html_url: "https://github.com/{owner}/{repo}/commit/abcd1234efgh5678ijkl9012mnop3456qrst7890"
      }
    ]
  }
];

const commit = mockCommitData[0].commit;

const fetchCommit = () => {
  // 模拟 API 调用，使用 mock 数据
  commit.value = mockCommitData[0].commit;
};

const goToGitHub = () => {
  window.open('https://github.com', '_blank');
};

watchEffect(() => {
    console.log('onMounted nextTick ')
})

onMounted(() => {
    // Vue.js 提供的一个延迟执行机制，可以在 DOM 更新完成后执行回调函数
    nextTick(() => {
      console.log('onMounted nextTick ')
    })
})

/** 测试跳转 changyou  */
function resetQuery() {

  proxy.$router.push({
        path: '/tool/gen-edit/c',
        query: {
            rowId: 1
        }
    });
}

</script>

<style scoped>

</style>
