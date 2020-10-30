<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{user.username}}</h1>
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile__create-tweet" @submit.prevent="createNewTweet()">
                <label for="newTweet"><strong>New Tweet</strong></label>
                <textarea id="newTweet" rows="4" v-model="newTweetContent" />

                <div class="user-profile__create-tweet-type">
                    <label for="newTweetType">Type:</label>
                    <select id="newTweetType" v-model="selectedTweetType">
                        <option :value="option.value" v-for="(option, index) in tweetTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>

                <button>Tweet!</button>
            </form>
        </div>

        <div class="user-profile__tweets-wrapper">
            <TweetItem 
                v-for="tweet in user.tweets" 
                :key="tweet.id" 
                :username="user.username" 
                :tweet="tweet" 
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
import TweetItem from './TweetItem'

export default {
  name: 'UserProfile',
  components: { TweetItem },
  data() {
    return {
      newTweetContent: '',
      selectedTweetType: 'instant',
      tweetTypes:[
          { value:'draft', name:'Draft'},
          { value:'instant', name:'Instant'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: '_MitchellRomney',
        firstName: 'Mitchell',
        lastName: 'Romney',
        email: 'mitchellromney@theearthsisquare.com',
        isAdmin: true,
        tweets: [
            {id:1, content: "Twitter is amazing!"},
            {id:2, content: "I like turtles!"},
        ]
      }
    }
  },
  watch: {
    followers(newFollwerCount, oldFollowerCount) {
      if(oldFollowerCount < newFollwerCount){
        console.log(`${this.user.username} has gained a follower!`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    }
  },
  methods: {
    followUser(){
      this.followers++;
    },
    toggleFavourite(id){
        console.log(`Favourited Tweet ${id}`);
    },
    createNewTweet(){
        console.log(this.selectedTweetType);
        if(this.newTweetContent && this.selectedTweetType != 'draft'){
            this.user.tweets.unshift({
                id: this.user.tweets.length +1,
                content: this.newTweetContent
            });
            this.newTweetContent = '';
        }
    }
  },
  mounted() {
    this.followUser();
  }
}
</script>

<style>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width:100%;
    padding:50px 5%
}
.user-profile__user-panel{
    display:flex;
    flex-direction:column;
    margin-right:50px;
    padding:20px;
    background-color: white;
    border-radius: 5px;
    border:1px solid #dfe3e8;
}
.user-profile__username{
    margin:0;
}
.user-profile__admin-badge{
    border-radius: 3px;
    background-color:rebeccapurple;
    color:white;
    padding:1px 10px;
    margin-right:auto;
    font-weight: bold;
    margin-top:10px;
    font-size:0.8rem;
}
.user-profile__follower-count{
    margin-top:5px;
}
.user-profile__create-tweet{
    margin-top:20px;
}
.user-profile__create-tweet label{
    display:block;
}
.user-profile__create-tweet textarea{
    width:100%;
}
</style>
