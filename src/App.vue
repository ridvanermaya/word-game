<template>
  <div id="app" class="container">
    <div class="card mt-4">
      <div class="card-header">
        Question
      </div>
      <div class="card-body">
        <div class="harf shadow">
          <span>X</span>
        </div>
      </div>
      <div class="card-footer">
        Functions
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  components: {
    
  },
  data() {
    return {
      questions: [],
      question: "",
      answer: "",
      letters: [],
      score: 0,
      letterScore: 0
    }
  }
}
</script>

<style>
.harf {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30pt;
}
</style>