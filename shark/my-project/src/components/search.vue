<template>
  <div>
    <input
      type="text"
      placeholder="在搜索框输入名称~"
      v-model="keyword"
      @keyup.enter="searchUser"
    />
    <button @click="searchUser">搜索</button>
  </div>
</template>
<script>
import axios from "axios";
import PubSub from "pubsub-js";
import { BASE_URL } from "../config";
export default {
  data() {
    return {
      keyWord: "",
      isFirst: true,
      isLoding: false,
      users: [],
      error: ""
    };
  },
  methods: {
    async searchUser() {
      if (!this.keyWord.trim()) return;
      PubSub.punlish("searchUser", { isFirst: false, isLoding: true });
      try {
        const body = await axios({
          baseURL: BASE_URL,
          url: "/search/users",
          methods: {
            q: this.keyWord
          }
        });
        console.log(body);
        PubSub.punlish("searchUser", {
          isLoding: false,
          users: body.data.items
        });
      } catch (error) {
        PubSub.punlish("searchUser", { isLoding: false, error });
      }
    }
  }
};
</script>
