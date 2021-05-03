<template>
  <div class="container mt-2">
    <h1 class="mb-3">Ask a Question</h1>
    <form @submit.prevent="onSubmit">
      <textarea
        v-model="question_body"
        class="form-control"
        placeholder="What do you want to ask?"
        rows="3"
      ></textarea>
      <br>
      <button
        type="submit"
        class="btn btn-success"
        >Publish
      </button>
    </form>
    <p v-if="error" class="muted mt-2">{{ error }}</p>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "QuestionEditor",
  props: {
    slug: {
      type: String,
      required: false
    }
  },
  data() {
    return {
      question_body: null,
      error: null,
    }
  },
  methods: {
    async onSubmit() {
    
    if (!this.question_body) {
        this.error = "You can't send an empty question!";
      } else if (this.question_body.length > 240) {
        this.error = "Ensure this field has no more than 240 characters!";
      } else {

        var data = { content: this.question_body}
        fetch('http://127.0.0.1:8000/api/questions/', {
           method: 'POST',
           headers: {
            'content-type': 'application/json',
            'X-CSRFTOKEN': '{{ csrf_token }}',
            'Authorization': 'Token 98d441bf8c4991fa7f1c99fe99a4f1d4cb14e4a5'
            
        },
        credentials: 'same-origin',
        body: JSON.stringify(data)
         })
        this.$router.push(`/question/${this.slug}/`)
      }
    }
  },
  created() {
    document.title = "Editor - QuestionTime";
  }  
}
</script>