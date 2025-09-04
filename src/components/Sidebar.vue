<template>
  <div class="Sidebar Sidebar--open">
    <PostForm v-if="isCreating" @created="$emit('created', $event)" @cancel="closeSidebar" />
    <PostPreview v-else-if="selectedPost" 
      :post="selectedPost" 
      @edit="editPost" 
      @delete="deletePost"
    />
    <CommentsList v-if="selectedPost" :postId="selectedPost.id" />
    <button class="button mt-3" v-if="selectedPost" @click="$emit('close')">Close Sidebar</button>
  </div>
</template>

<script>
import PostForm from './PostForm.vue';
import PostPreview from './PostPreview.vue';
import CommentsList from './CommentsList.vue';

export default {
  props: {
    selectedPost: Object,
    isCreating: Boolean
  },
  components: { PostForm, PostPreview, CommentsList },
  methods: {
    closeSidebar() {
      this.$emit('close');
    },
    editPost(post) {
      this.$emit('updated', post);
    },
    deletePost(postId) {
      this.$emit('deleted', postId);
    }
  }
};
</script>

<style scoped>
.Sidebar { width: 400px; background: #f5f5f5; padding: 1rem; position: fixed; right: 0; top: 0; bottom: 0; overflow-y: auto; }
.Sidebar--open { display: block; }
</style>
