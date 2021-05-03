<template>
  <div class="single-question mt-2">
    <div class="container">
      <h1>{{ question.content }}</h1>
      <p class="mb-0">Posted by:
        <span class="author-name">{{ question.author }}</span>
      </p>
      <p>{{ question.created_at }}</p>
      <hr>

      <div v-if="userHasAnswered">
        <p class="answer-added">You've written an answer!</p>
      </div>
      <div v-else-if="showForm">
        <form class="card" @submit.prevent="onSubmit">
          <div class="card-header px-3">
            Answer the Question
          </div>
          <div class="card-block">
            <textarea 
              v-model="newAnswerBody"
              class="form-control"
              placeholder="Share Your Knowledge!"
              rows="5"
            ></textarea>
          </div>
          <div class="card-footer px-3">
            <button type="submit" class="btn btn-sm btn-success">Submit Your Answer</button>
          </div>
        </form>
        <p v-if="error" class="error mt-2">{{ error }}</p>
      </div>
      <div v-else>
        <button
          class="btn btn-sm btn-success"
          @click="showForm = true"
          >Answer the Question
        </button>
      </div>
      <hr>

    </div>
      <AnswerComponent 
        v-for="(answer, index) in answers"
        :answer="answer"
        :key="index"
      />
  </div>
</template>

<script>
import axios from 'axios'
import AnswerComponent from "@/components/Answer.vue";

export default {
  name: "Question",
  props: {
    slug: {
      type: String,
      required: true
    }
  },
  components: {
    AnswerComponent
  },
  data() {
    return {
      question: {},
      answers: [],
      newAnswerBody: null,
      error: null,
      userHasAnswered: false,
      showForm: false,
      requestUser: null
    }
  },
  methods: {
    setPageTitle(title) {
      document.title = title;
    },
    setRequestUser() {
      this.requestUser = window.localStorage.getItem("username");
    },
    async getQuestionData() {
      try { 
          const response = await axios.get(`/api/questions/${this.slug}/`)
          this.question = response.data
          this.userHasAnswered = response.data.user_has_answered
          this.setPageTitle(response.data.content)
          } catch(error){
              console.log(error)
          }
    },
    async getQuestionAnswers() {
      try { 
          const response = await axios.get(`/api/questions/${this.slug}/answers/`)
          this.answers = response.data
          } catch(error){
              console.log(error)
          }
    },


    async onSubmit() {
      if (this.newAnswerBody) {
        try{
          var data = { body: this.newAnswerBody}
          fetch(`http://127.0.0.1:8000/api/questions/${this.slug}/answer/`, {
             method: 'POST',
             headers: {
              'content-type': 'application/json',
              'X-CSRFTOKEN': '{{ csrf_token }}',
              'Authorization': 'Token 98d441bf8c4991fa7f1c99fe99a4f1d4cb14e4a5'    
          },
          credentials: 'same-origin',
          body: JSON.stringify(data)
          })
          this.answers.unshift(data)
          this.newAnswerBody = null;
          this.showForm = false;
          this.userHasAnswered = true;
        } catch(error) {
              console.log(JSON.stringify(error))
          }
      } 
    }
  },
  created() {
    this.getQuestionData()
    this.getQuestionAnswers()
    this.setRequestUser()
  }
}
</script>
