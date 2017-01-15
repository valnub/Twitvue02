<template lang="jade">
  div#app
    f7-login-screen#loginScreen
      f7-view
        f7-pages
          f7-page.loginPageScreen(login-screen='')
            f7-login-screen-title Twitvue
            img.twitlogo(src="gfx/twitter.png")
            f7-list
              f7-list-button(title='Sign In', v-on:click="onSignIn")
              f7-list-label
                p Click Sign In to log in to Twitter
    f7-views(navbar-through='')
      f7-view#mainView(main='', url='/', :dynamic-navbar='true')
        f7-navbar
          f7-nav-left
          f7-nav-center(sliding='') Twitvue
          f7-nav-right
        f7-pages#pages
          f7-page.navbar-fixed
            f7-searchbar(cancel-link='Cancel', placeholder='Search in items', :clear='true', v-on:change="onChange")
            f7-list.tweets(media-list='')
              f7-list-item(v-on:click='onClick(tweet)', link='/tweet/', v-for='tweet in tweets', :media='tweet | imgFilter', :title='tweet | userFilter', :subtitle='tweet | screenNameFilter', :text='tweet.text')
</template>

<script>
import twitter from './twitter.js';
import store from './store.js';
import filters from './filters.js';

let self;

export default {
  name: 'app',
  data () {
    return {
      tweets : store.tweets
    }
  },
  created () {
    self = this;
  },
  filters : filters,
  methods : {
    onSignIn () {
      window.f7.showPreloader();
      twitter.login();
    },
    onChange (event) {
      let term = event.target.value;
      if (term) {
        window.f7.showPreloader();
        twitter.cb.__call(
            "search_tweets",
            "q=" + term,
            function (reply) {
                let result = reply.statuses;
                store.searchResults.splice(0, store.searchResults.length);
                store.searchResults.push(...result);
                
                var mainView = Dom7('#mainView')[0].f7View;
                mainView.router.load({url: '/search/'});
                
                window.f7.hidePreloader();
            },
            true // this parameter required
        );
      }
    },
    onClick: function (tweet) {
      store.selectedTweet = tweet;
    }
  }
}
</script>

<style lang="sass?indentedSyntax">
  .tweets
    .item-media
      img
        border-radius: 100%;
  
  .loginPageScreen
    text-align: center;
  
  .twitlogo
    width: 70px
</style>
