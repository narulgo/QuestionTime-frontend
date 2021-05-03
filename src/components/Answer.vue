<template>
  <div class="single-answer" v-for="i in answer" :key="i">
      <strong>{{ i.author }}</strong> {{ i.created_at }}
    <p>{{ i.body }}</p>
  </div>
</template>

<script>
export default {
  name: "AnswerComponent",
  props: {
    answer: {
      type: Object,
      required: true
    }
  }
}
</script>