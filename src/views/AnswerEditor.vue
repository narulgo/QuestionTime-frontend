<template>
  <div class="container mt-2">
    <h1 class="mb-3">Edit Your Answer</h1>
    <form @submit.prevent="onSubmit">
      <textarea 
        v-model="answerBody" 
        class="form-control" 
        rows="3"
      ></textarea>
      <br>
      <button 
        type="submit" 
        class="btn btn-success"
        >Publish your answer
      </button>
    </form>
    <p v-if="error" class="muted mt-2">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "AnswerEditor",
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      questionSlug: null,
      answerBody: null,
      error: null
    }
  },
  methods: {
    async onSubmit() {
      if (this.answerBody) {
        const response = await axios.patch(`/api/answers/${this.id}/`, { body: this.answerBody })
      }
    }
  }
}
</script>