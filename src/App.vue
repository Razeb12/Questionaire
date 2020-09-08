<template>
  <div class="app">
    <video autoplay muted loop id="myVideo">
      <source src="./assets/Particles - 323.mp4" type="video/mp4" />
    </video>
    <Header></Header>
    <div class="container mt-5 video">
      <div class="row">
        <div class="col-sm-12">
          <Questionaire
          :numCorrect="numCorrect" :numTotal="numTotal"
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          ></Questionaire>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Questionaire from "./components/Questionaire.vue";

export default {
  name: "App",
  components: {
    Header,
    Questionaire,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 20,
    };
  },
  methods: {
    next() {
      {
      this.index++;
      }
    },
    increment() {
       {
        this.numCorrect++;
      }
    },
  },
  
  
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&category=17&type=multiple", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Recursive&display=swap");
* {
  margin: 0;
  padding: 0;
  font-family: 'Recursive', sans-serif;
}
.question-box {
  margin-left: auto;
  margin-right: auto;
}
.app {
  background-size: cover;
  background-repeat: no-repeat;
  width: auto;
  height: 100vh;
}
#myVideo {
  position: absolute;
  right: 0;
  bottom: 0;
  padding: 0;
  min-width: 100%;
  min-height: 100%;
}
</style>
