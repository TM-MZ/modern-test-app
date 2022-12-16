<template>
  <div class="container">
    <h1>独り言App</h1>
    <div class="form">
      <p>つぶやく内容を入力してください</p>
      <div>
        <validation-Observer ref="obs" v-slot="ObserverProps">
          <validation-provider v-slot="{ errors }" rules="required|max:140">
            <div class="error">{{ errors[0] }}</div>
            <input type="text" name="つぶやき" id="content" v-model="content" />
          </validation-provider>
          <button @click="insertTweet" :disabled="ObserverProps.invalid || !ObserverProps.validated">つぶやく</button>
        </validation-Observer>
      </div>
    </div>
    <h2>つぶやき一覧</h2>
    <div class="message">
      <Tweet
        v-for="tweet in tweets"
        :key="tweet.id"
        :tweet="tweet"
        @deleteTweet="deleteTweet"
      ></Tweet>
    </div>
  </div>
</template>

<script>
import Tweet from "../components/Tweet.vue";
export default {
  components: { Tweet },
  data() {
    return {
      tweets: [],
      content:"",
    };
  },
  methods: {
    //つぶやきをすべて取得する
    async getTweets() {
      const resData = await this.$axios.get(
        "http://127.0.0.1:8000/api/v1/tweet"
      );
      if (resData) {
        this.tweets = resData.data.data;
      }
      console.log(this.tweets);
    },
    //つぶやきを追加する
    async insertTweet() {
      const sendData = {
        content: this.content,
      };
      const data = await this.$axios.post(
        "http://127.0.0.1:8000/api/v1/tweet",
        sendData
      );
      if (data) {
        alert("つぶやきました");
        this.$router.go({ path: "/", force: true });
      }
    },
    //つぶやきを削除する（emitを使用)
    async deleteTweet(id) {
      await this.$axios
        .delete("http://127.0.0.1:8000/api/v1/tweet/" + id)
        .then(() => {
          alert("削除しました");
                  this.$router.go({ path: "/", force: true });
        });
    },
  },
  created() {
    this.getTweets();
  },
};
</script>
<style>
* {
  font-family: sans-serif;
}
.container {
  width: 500px;
  text-align: center;
  margin: 0 auto;
}
.form {
  margin: 50px 0;
}
.message {
  border: 1px solid black;
}
.error{
  color:red;
}
</style>
