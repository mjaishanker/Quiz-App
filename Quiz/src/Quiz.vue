<template>
  <div class="container-app">
    <div class="container-quiz">
      <div class="quiz-header">
        <h1>Quiz Vue App</h1>
      </div>
      <div
        class="quiz-main"
        v-for="(element, index) in questions.slice(a, b)"
        :key="index"
        v-show="quiz"
      >
        <div class="box-question">
          <h2>Question</h2>
          <p>{{ element.question }}</p>
        </div>
        <div id="listAnswers" class="box-answers">
          <ul>
            <li
              v-for="(item, index) in element.answers"
              :key="index"
              :class="select ? checkAnswer(item) : ''"
              @click="selectResponse(item)"
            >
              {{ item.text }}
            </li>
          </ul>
        </div>
      </div>

      <div class="box-score" v-if="scores_show">
        <div class="result-grid" v-if="results_show">
          <div v-for="ques in resultsClickedFin" v-bind:key="ques">
            {{ ques }}
          </div>
        </div>
        <h2>Score:</h2>
        <h2>{{ score }}/{{ questions.length }}</h2>
      </div>

      <div class="quiz-footer">
        <div class="box-button">
          <button v-if="next_button_show" @click="nextQuestion">Next</button>
          <button v-if="results_show" @click="restart">Restart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Quiz",
  data() {
    return {
      questions: [
        {
          question: "What is 7 + 3?",
          answers: [
            { text: "10", correct: true },
            { text: "22" },
            { text: "73" },
            { text: "37" },
          ],
        },
        {
          question: "How many rings are on the Olympic flag?",
          answers: [
            { text: "None" },
            { text: "4" },
            { text: "5", correct: true },
            { text: "7" },
          ],
        },
        {
          question: "What is the function of mitochondria in the cell?",
          answers: [
            { text: "Kill Antigens" },
            { text: "Process Waste" },
            { text: "Repair Damage" },
            { text: "Generate Energy", correct: true },
          ],
        },
        {
          question: "What is the largest moon in the Solar System?",
          answers: [
            { text: "Our Moon" },
            { text: "Pluto" },
            { text: "Ganymede", correct: true },
            { text: "Titan" },
          ],
        },
        {
          question: "The science of weights and measures is called:",
          answers: [
            { text: "Metrology", correct: true },
            { text: "Meteorology" },
            { text: "Mineralogy" },
            { text: "Morphology" },
          ],
        },
      ],

      a: 0,
      b: 1,
      select: false,
      score: 0,
      quiz: true,
      scores_show: false,
      results_show: false,
      next_button_show: false,
      resultsClickedInd: [],
      resultsClickedFin: [],
    };
  },
  methods: {
    selectResponse(e) {
      this.select = true;

      //console.log("item", e.text);
      //console.log("question", this.questions[this.a].question);
      //console.log("correct", e.correct);
      this.resultsClickedInd.push(this.questions[this.a].question);
      this.resultsClickedInd.push(e.text);
      this.next_button_show = true;
      //console.log(this.resultsClicked.questions);
      if (e.correct) {
        this.resultsClickedInd.push("Correct");
      } else {
        this.resultsClickedInd.push("Incorrect");
      }
    },

    checkAnswer(status) {
      if (status.correct) {
        return "correct";
      } else {
        return "wrong";
      }
    },
    nextQuestion() {
      this.resultsClickedFin.push(this.resultsClickedInd[0]);
      this.resultsClickedFin.push(this.resultsClickedInd[1]);
      this.resultsClickedFin.push(this.resultsClickedInd[2]);
      this.resultsClickedInd = [];
      console.log(this.resultsClickedFin);
      if (this.questions.length - 1 == this.a) {
        this.scores_show = true;
        this.results_show = true;
        this.next_button_show = false;
        this.quiz = false;

        for (var i = 0; i < this.resultsClickedFin.length; i++) {
          if (this.resultsClickedFin[i] == "Correct") {
            this.score++;
          }
        }
        console.log(this.score);
      } else {
        this.a++;
        this.b++;
        this.select = false;
        this.next_button_show = false;
        console.log(this.score);
      }
    },

    restart() {
      Object.assign(this.$data, this.$options.data());
    },
  },
};
</script>
