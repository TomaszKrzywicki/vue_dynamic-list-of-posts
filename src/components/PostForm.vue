<template>
  <form @submit.prevent="handleSubmit">
    <div class="field">
      <label class="label">Title</label>
      <div class="control">
        <input class="input" v-model="title" required />
      </div>
    </div>

    <div class="field">
      <label class="label">Body</label>
      <div class="control">
        <textarea class="textarea" v-model="body" required></textarea>
      </div>
    </div>

    <div class="buttons">
      <button class="button is-primary" type="submit">
        {{ post ? 'Save' : 'Create' }}
      </button>
      <button class="button" type="button" @click="$emit('cancel')">Cancel</button>
    </div>
  </form>
</template>

<script>
export default {
  props: {
    post: Object,
  },
  data() {
    return {
      title: this.post?.title || '',
      body: this.post?.body || '',
    };
  },
  methods: {
    async handleSubmit() {
      const payload = {
        title: this.title,
        body: this.body,
        userId: 1,
      };

      const url = this.post
        ? `https://mate-academy.github.io/fe-students-api/posts/${this.post.id}`
        : 'https://mate-academy.github.io/fe-students-api/posts';

      const method = this.post ? 'PATCH' : 'POST';

      const response = await fetch(url, {
        method,
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload),
      });

      const result = await response.json();
      this.$emit(this.post ? 'updated' : 'created', result);
    },
  },
};
</script>
