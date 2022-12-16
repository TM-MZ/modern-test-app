<template>
  <div class="container">
    <div class="left">
    <div class="circle"></div>
    <p class="tweet">{{tweet.content}}</p>
    </div>
    <button @click="deleteTweet">削除</button>
  </div>
</template>

<script>
export default {
  props: {
    tweet:Object
  },
  data() {
    return {};
  },
  methods: {
    deleteTweet() {
        this.$emit('deleteTweet',this.tweet.id);
    },
  },
};
</script>

<style scoped>
.container {
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 0 10px;
}
.left{
    display: flex;
    justify-content: center;
    align-items: center;
}
.tweet {
    text-align: left;
    width:350px;
}
.circle {
  top: 15px;
  width: 40px;
  height: 40px;
  background-color: red;
  border: none;
  border-radius: 50px;
  margin-right: 10px;
}
</style>
