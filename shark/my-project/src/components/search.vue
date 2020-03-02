<template>
  <div>
    <input
      type="text"
      placeholder="在搜索框输入名称~"
      v-model="keyWord"
      @keyup.enter="searchUser"
    
    />
    <button @click="searchUser">搜索</button>
  </div>
</template>
<script>
//  @keyup.enter="searchUser"
//      输入框绑定回车事件 
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
      PubSub.publish("searchUser", { isFirst: false, isLoding: true });
      try {
        const body = await axios({
          baseURL: BASE_URL,
          url: "/search/users",
          methods: "get", //get请求的query参数，在axios要写成  params形式
          params: { q: this.keyWord } //q是规定要传的参数
        });
        console.log(body);
        PubSub.publish("searchUser", {
          isLoding: false,
          users: body.data.items
        });
      } catch (error) {
        PubSub.publish("searchUser", { isLoding: false, error });
      }
    }
  }
};
</script>
