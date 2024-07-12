<template>
    <div class="container mx-auto">
      <div v-if="loading" class="text-center mt-4">Loading tweets...</div>
      <div v-else>
        <div v-for="tweet in tweets" :key="tweet.id" class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
          <div class="flex justify-between items-center">
            <div class="flex items-center">
              <img :src="tweet.avatar" class="w-10 h-10 rounded-full mr-4">
              <div class="text-sm">
                <p class="text-gray-900 leading-none">{{ tweet.name }}</p>
                <p class="text-gray-600">{{ '@' + tweet.username }}</p>
              </div>
            </div>
            <div class="text-gray-600 text-xs">{{ formatDate(tweet.createdAt) }}</div>
          </div>
          <div class="mt-4">
            <p class="text-gray-700">{{ tweet.content }}</p>
          </div>
          <div class="mt-4 flex">
            <div class="flex-1 text-center">
              <a href="#" @click.prevent="toggleLike(tweet)">
                <svg :class="{ 'text-red-500': tweet.liked, 'text-gray-500': !tweet.liked }" class="h-6 w-6 inline-block" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" stroke="currentColor" viewBox="0 0 24 24">
                  <path d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path>
                </svg>
              </a>
            </div>
            <div class="flex-1 text-center">
              <!-- Add other icons or actions here -->
            </div>
          </div>
        </div>
        <div class="mt-4">
          <form @submit.prevent="postTweet">
            <textarea v-model="newTweetContent" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" rows="3" placeholder="What's happening?"></textarea>
            <button type="submit" class="mt-2 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
              Tweet
            </button>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        tweets: [],
        loading: false,
        newTweetContent: ''
      };
    },
    methods: {
      fetchTweets() {
        this.loading = true;
        axios.get('http://localhost:3000/tweets')
          .then(response => {
            this.tweets = response.data;
            this.loading = false;
          })
          .catch(error => {
            console.error('Error fetching tweets:', error);
            this.loading = false;
          });
      },
      postTweet() {
        const newTweet = {
          id: this.tweets.length + 1, // Replace with appropriate ID generation logic
          name: 'John Doe', // Replace with actual user data
          username: 'johndoe', // Replace with actual user data
          avatar: 'https://randomuser.me/api/portraits/men/1.jpg', // Replace with actual user data
          content: this.newTweetContent,
          createdAt: new Date().toISOString(),
          liked: false // Initialize liked state
        };
  
        axios.post('http://localhost:3000/tweets', newTweet)
          .then(response => {
            this.tweets.unshift(newTweet); // Add new tweet to the beginning of the list
            this.newTweetContent = ''; // Clear input field
          })
          .catch(error => {
            console.error('Error posting tweet:', error);
          });
      },
      toggleLike(tweet) {
        tweet.liked = !tweet.liked; // Toggle the liked state
      },
      formatDate(dateString) {
        // Format date logic here (if needed)
        return dateString;
      }
    },
    mounted() {
      this.fetchTweets(); // Fetch tweets when component is mounted
    }
  };
  </script>
  
  <style scoped>
  /* Add your scoped styles here */
  </style>  