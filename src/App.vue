<template>
  <div class="container column">
    <app-form @action="addSection"></app-form>

    <div id="content" class="card card-w70">
      <KeepAlive>
        <component v-for="section in content" :is="section.section" :key="section" :value="section.value"></component>
      </KeepAlive>
    </div>
  </div>
  <div class="container">
    <p>
      <button class="btn primary" @click="loadComments">Загрузить комментарии</button>
    </p>
    <app-comments v-if="comments.length !== 0" :comments="comments"></app-comments>
    <app-loader v-if="loader === true"></app-loader>
  </div>
</template>

<script>
import AppAvatar from './components/AppAvatar.vue';
import AppComments from './components/AppComments.vue';
import AppForm from './components/AppForm.vue';
import AppHeader from './components/AppHeader.vue';
import AppLoader from './components/AppLoader.vue';
import AppSubheader from './components/AppSubheader.vue';
import AppText from './components/AppText.vue';


export default {
  data() {
    return {
      loader: false,
      comments: [],
      content: []
    }
  },
  methods: {
    addSection(type, value) {
      switch(type) {
        case 'title': {
          this.content.push({section: AppHeader, value: value});
          break;
        }
        case 'subtitle': {
          this.content.push({section: AppSubheader, value: value});
          break;
        }
        case 'avatar': {
          this.content.push({section: AppAvatar, value: value});
          break;
        }
        case 'text': {
          this.content.push({section: AppText, value: value});
          break;
        }
      }
    },
    async loadComments() {
      this.loader = true;
      const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42', {
        method: 'GET',
        headers: {
          'Content-type': 'application/json'
        }
      });
      this.loader = false;
      const data = await response.json();
      this.comments = data.map(comment => {
        return {
          id: comment.id,
          body: comment.body,
          email: comment.email
        }
      })
    }
  },
  components: {
    AppComments,
    AppLoader,
    AppHeader,
    AppAvatar,
    AppSubheader,
    AppText,
    AppForm
  }

}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
