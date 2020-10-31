<template>
  <div class="tweet-item" @click="favouriteTweet(tweet.id)">
    <div class="tweet-item__user">@{{ username }}</div>
    <div class="tweet-item__content">
      {{ tweet.content }}
    </div>
  </div>
</template>

<script>
export default {
  name: "TweetItem",
  props: {
    username: {
      type: String,
      required: true,
    },
    tweet: {
      type: Object,
      required: true,
    },
  },
  methods: {
    favouriteTweet(id) {
      this.$emit("favourite", id);
    },
  },
};
</script>

<style scoped lang="scss">
.tweet-item {
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  box-sizing: border-box;
  cursor: pointer;
  transition: all 0.25s ease;

  &:hover {
    transform: scale(1.1, 1.1);
  }

  .tweet-item__user {
    font-weight: bold;
  }
  
  .tweet-item__content {
    margin-top: 1rem;
  }
}
</style>