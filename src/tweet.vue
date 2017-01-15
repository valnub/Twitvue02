<template lang="jade">
  f7-page(hide-bars-on-scroll='')
    f7-navbar(back-link='Back', sliding='')
    .card.facebook-card.tweet
      .card-header.no-border
        .facebook-avatar
          img(:src='tweet | imgUrlFilter', width='34', height='34')
        .facebook-name {{tweet | userFilter}}
        .facebook-date {{tweet | dateFilter}}
      .card-content
        {{tweet.text}}
        img(v-if="tweet.entities.media", :src='tweet.entities.media[0].media_url', width='100%')
      .card-footer.no-border
        a.link(href='#') Like
        a.link(href='#') Comment
        a.link(href='#') Share

</template>

<script>
import store from './store.js';
import filters from './filters.js'

export default {
  name: 'tweet',
  filters: filters,
  data () {
    return {
      tweet: store.selectedTweet
    }
  }
}
</script>

<style lang="sass?indentedSyntax">
  .tweet
    .card-content
      padding: 15px;
</style>
