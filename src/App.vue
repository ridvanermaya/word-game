<template>
  <div id="app" class="container">
    <div class="card mt-4" v-if="!currentQuestion">
      <div class="card-body">
        To start the game please click "Start Game" button.
      </div>
      <div class="card-footer">
        <button class="btn btn-primary" @click="startGame">Start Game</button>
      </div>
    </div>
    <div class="card mt-4" v-else>
      <div class="card-header">{{ currentQuestion.question }}</div>
      <div class="card-body">
        <div class="d-flex">
          <div class="harf shadow mr-3" v-for="(letter, index) in letters" :key="'letter-'+index">
            <span v-if="letter.shown">{{ letter.letter }}</span>
            <span v-else></span>
          </div>
        </div>
      </div>
      <div class="card-footer">Word Score: {{ letterScore }}</div>
      <div class="card-footer">Total Score: {{ score }}</div>
      <div class="card-footer">
        <p>
          <input type="text" class="form-control" placeholder="Type your answer" v-model="playerAnswer">
        </p>
        <p>
          Your Answer: {{ playerAnswer}}
        </p>
        <button @click="answer">Answer</button>
      </div>
      <div class="card-footer">
        <button class="btn btn-secondary" @click="giveLetter">Give Hint</button>
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
      questions: [
        {
          question: "On top of the house",
          answer: "ROOF",
          asked: false
        },
        {
          question: "Usually cut in slices",
          answer: "BREAD",
          asked: false
        },
        {
          question: "A dairy product",
          answer: "BUTTER",
          asked: false
        },
        {
          question: "A top with long sleeves, often in wool",
          answer: "SWEATER",
          asked: false
        }
      ],
      currentQuestion: null,
      letters: [],
      score: 0,
      letterScore: 0,
      playerAnswer: ""
    }
  },
  methods: {
    startGame() {
      this.currentQuestion = this.questions.find(x => !x.asked);
      this.letters = [];
      this.currentQuestion.answer.split("").map(x => {
        this.letters.push({
          letter: x,
          shown: false
        });
      });
      this.letterScore = this.letters.length * 100;
    },
    giveLetter() {
      let randomLetterIndex = Math.floor(Math.random() * this.letters.length);

      if(this.letterScore <= 100) {
        return;
      }

      let letter = this.letters[randomLetterIndex];

      while (letter.shown) {
        randomLetterIndex = Math.floor(Math.random() * this.letters.length);
        letter = this.letters[randomLetterIndex];
      }

      letter.shown = true;
      this.letterScore -= 100;
    },
    answer() {
      alert(this.playerAnswer);
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