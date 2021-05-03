<template>
  <div class="home">
    <div class="container">
      <div v-for="question in questions.results"
           :key="question.pk">
        <p class="mb-0">Posted by:
          <span class="author-name">{{ question.author }}</span>
        </p>
        <h2>
        <router-link :to="{ name: 'question', params: { slug: question.slug } }" class="question-link">
        {{ question.content }}
        </router-link>
        </h2>
        <p>Answers: {{ question.answers_count }}</p>
        <hr>
      </div>
      <div class="my-4">
        <p v-show="loadingQuestions">...loading...</p>
        <button
          v-show="next"
          @click="getQuestions"
          class="btn btn-sm btn-outline-success"
          >Load More
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "Home",
  data() {
    return {
      questions: [],
      next: null,
      loadingQuestions: false,
    }
  },
  methods: {
    async getQuestions() {
      try { 
          const response = await axios.get('/api/questions/')
          if (this.next) {
            response = this.next;
            }
          this.loadingQuestions = true;
          this.questions = response.data
          this.loadingQuestions = false;
          if (data.next) {
            this.next = data.next;
          } else {
            this.next = null;
          }
          } catch(error){
              console.log(error)
          }
    }
  },
  created() {
    this.getQuestions()
    document.title = "QuestionTime";
  }
}
</script>