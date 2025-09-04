<template>
  <form @submit.prevent="submitComment" class="mt-3">
    <div class="field">
      <label class="label">Name</label>
      <div class="control">
        <input class="input" v-model="name" :class="{ 'is-danger': errors.name }" @input="clearError('name')" required />
      </div>
      <p v-if="errors.name" class="help is-danger">{{ errors.name }}</p>
    </div>

    <div class="field">
      <label class="label">Email</label>
      <div class="control">
        <input type="email" class="input" v-model="email" :class="{ 'is-danger': errors.email }" @input="clearError('email')" required />
      </div>
      <p v-if="errors.email" class="help is-danger">{{ errors.email }}</p>
    </div>

    <div class="field">
      <label class="label">Comment</label>
      <div class="control">
        <textarea class="textarea" v-model="body" :class="{ 'is-danger': errors.body }" @input="clearError('body')" required></textarea>
      </div>
      <p v-if="errors.body" class="help is-danger">{{ errors.body }}</p>
    </div>

    <div class="buttons mt-2">
      <button class="button is-primary" :class="{ 'is-loading': isSubmitting }" type="submit">Submit</button>
      <button class="button" type="button" @click="clearForm">Clear</button>
    </div>
  </form>
</template>

<script>
export default {
  props: { postId: Number },
  data() {
    return { name: '', email: '', body: '', errors: {}, isSubmitting: false };
  },
  methods: {
    clearError(field) { this.errors[field] = null; },
    clearForm() { this.name = ''; this.email = ''; this.body = ''; this.errors = {}; },
    async submitComment() {
      this.errors = {};
      if (!this.name) this.errors.name = 'Name is required';
      if (!this.email) this.errors.email = 'Email is required';
      if (!this.body) this.errors.body = 'Comment is required';
      if (Object.keys(this.errors).length) return;

      this.isSubmitting = true;
      try {
        const res = await fetch('https://mate-academy.github.io/fe-students-api/comments', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ name: this.name, email: this.email, body: this.body, postId: this.postId }),
        });
        const newComment = await res.json();
        this.$emit('added', newComment);
        this.body = '';
      } catch {
        this.errors.general = 'Failed to submit comment';
      } finally { this.isSubmitting = false; }
    }
  }
};
</script>
