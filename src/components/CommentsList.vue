<template>
  <div class="mt-4">
    <Loader v-if="loading" />
    <Notification v-if="error" :message="error" type="is-danger" />
    <NoCommentsYet v-if="!comments.length && !loading" />

    <div v-for="comment in comments" :key="comment.id" class="box">
      <p><strong>{{ comment.name }}</strong> ({{ comment.email }})</p>
      <p>{{ comment.body }}</p>
      <button class="button is-small is-danger" @click="deleteComment(comment.id)">Delete</button>
    </div>

    <CommentForm @added="addComment" :postId="postId" />
  </div>
</template>

<script>
import Loader from './Loader.vue';
import Notification from './Notification.vue';
import CommentForm from './CommentForm.vue';
import NoCommentsYet from './NoCommentsYet.vue';

export default {
  props: { postId: Number },
  components: { Loader, Notification, CommentForm, NoCommentsYet },
  data() {
    return { comments: [], loading: false, error: null };
  },
  methods: {
    async fetchComments() {
      this.loading = true;
      try {
        const res = await fetch(`https://mate-academy.github.io/fe-students-api/comments?postId=${this.postId}`);
        this.comments = await res.json();
      } catch {
        this.error = 'Failed to load comments';
      } finally {
        this.loading = false;
      }
    },
    deleteComment(id) { this.comments = this.comments.filter(c => c.id !== id); },
    addComment(comment) { this.comments.push(comment); }
  },
  mounted() { this.fetchComments(); }
};
</script>
