<template>
  <div id="app" class="container mt-4">
    <div class="card mb-4" v-if="completed">
      <div class="card-body">
        Congratulations you completed the game with the score of {{ this.score }}
      </div>
    </div>
    <div class="card" v-if="!currentQuestion">
      <div class="card-body">
        To start the game please click "Start Game" button.
      </div>
      <div class="card-footer">
        <button class="btn btn-primary" @click="startGame">Start Game</button>
      </div>
      
    </div>
    <div class="card" v-else>
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
          Remaining Time: <kbd>{{ this.remainingTime }}</kbd> seconds
        </p>
        <p>
          <input type="text" class="form-control" placeholder="Type your answer" v-model="playerAnswer" @keyup="playerAnswer = playerAnswer.toUpperCase()">
        </p>
        <p>
          Your Answer: {{ playerAnswer}}
        </p>
        <button class="btn btn-success" @click="answer">Answer</button>
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
      playerAnswer: "",
      completed: false,
      time: null,
      remainingTime: 0
    }
  },
  methods: {
    startGame() {
      this.currentQuestion = null;
      this.completed = false;
      this.score = 0;
      this.questions.map(x => x.asked = false);
      this.remainingTime = 240;
      this.time = setInterval(() => {
        this.remainingTime--;
        
        if (this.remainingTime === 0) {
          this.finish();
        }
      }, 1000);

      this.giveQuestion();
    },
    giveQuestion() {
      this.playerAnswer = "";
      this.currentQuestion = this.questions.find(x => !x.asked);

      if(!this.currentQuestion) {
        this.finish();
      }

      this.letters = [];
      this.currentQuestion.answer.split("").map(x => {
        this.letters.push({
          letter: x,
          shown: false
        });
      });
      this.letterScore = this.letters.length * 100;
      this.currentQuestion.asked = true;
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
      let answer = this.playerAnswer.toUpperCase();
      this.playerAnswer = answer;

      if (this.playerAnswer === this.currentQuestion.answer.toUpperCase()) {
        this.score += this.letterScore;
      } else {
        this.score -= this.letterScore;
      }

      this.giveQuestion();
    },
    finish() {
      clearInterval(this.time);
      this.currentQuestion = null;
      this.completed = true;
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