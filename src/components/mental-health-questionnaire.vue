<template lang="pug">
  div
    #mental-health-questionnaire(:class="outerClass")
      // intro screen
      .intro(v-if="!quizInProgress" @click="startQuiz")
        img(src="img/which-career-in-mental-health-is-right-for-you.png" alt="Which career in mental health is right for you?")
        .lg.gray.bold So you’re interested in working in mental health, but not sure in what capacity?
        .lg.gray This short quiz will suggest some job roles that you may not have considered before, based on the answers that you give. Click each job role to find out more.
      // quiz
      .inner(v-else)
        .left
          mental-health-jobs(v-for="i in 3" :key="`q-${i - 1}`" :color="colors[i - 1]" :jobs="getJobsForGroup(i - 1)" :group="groups[i - 1]")
        .mid
          // questions
          .questions(v-if="!quizOver")
            .question-wrapper
              // .question-number Question {{ questionNumber + 1 }}:
              .question-content
                .icon
                  img(:src="`img/q${questionNumber + 1}-icon.png`")
                .question-text-wrapper
                  .question-text
                    .question-title {{ questions[questionNumber].headline }}
                    .question-sub-title(v-if="questions[questionNumber].subtitle") {{ questions[questionNumber].subtitle }}
            .question-answer-options
              button.questionnaire-button.yes(@click="yesOnClick") YES
              button.questionnaire-button.no(@click="noOnClick") NO
          // thank you message
          .thank-you(v-else)
            .thank-you-wrapper
              img(src="img/thank-you-for-completing-the-quiz.png" alt="Thank you for completing our quiz")
              .lg.gray The job roles that are still highlighted are the ones that best suit you, based on your answers provided.
              .lg.gray.bold Click any role to find out more.
        .right
          mental-health-jobs(v-for="i in 5" :key="`q-${i + 2}`"  :color="colors[i + 2]" :jobs="getJobsForGroup(i + 2)" :group="groups[i + 2]")
</template>

<script>
import MentalHealthJobs from './mental-health-jobs'
import axios from 'axios'

//const data = () => import(/* webpackChunkName: 'Anything' */ '../data.js')

export default {
  name: 'mental-health-questionnaire',
  components: {
    MentalHealthJobs
  },
  data() {
    return {
      jobs: [],
      quizInProgress: false,
      quizOver: false,
      questionNumber: 0,
      questions: [
        { // 0
          headline: `Do you have an undergraduate degree or are you prepared to study at an undergraduate level?`,
        },
        { // 1
          headline: `Do you have a medical degree or are you interested in undertaking a medical degree?`,
          prereqs: 0,
        },
        { // 2
          headline: `Do you have a psychology degree`,
          prereqs: 0
        },
        { // 3
          headline: `Are you prepared to study at a post-graduate level?`,
          prereqs: 0
        },
        { // 4
          headline: `Are you working in a core profession* at the moment?`,
          subtitle: `*This includes, but is not limited to: applied psychology, nursing, medicine, and allied health professions.`,
          prereqs: 0
        },
        { // 5
          headline: `Do you have lived experience of mental health*? `,
          subtitle: `*Lived experience means you are living or have lived with mental illness, or you support or have supported someone living with mental illness.`,
        },
        { // 6
          headline: `Are you interested in mostly working with adult patients?`,
        },
        { // 7
          headline: `Do you have a background or experience in art, music or drama?`,
          prereqs: 3
        },
      ],
      groups: [
        { margin: 14, padding: 6 },
        { margin: 12, padding: 7 },
        { margin: 12, padding: 6 },
        { margin: 14, padding: 5 },
        { margin: 5, padding: 8 },
        { margin: 5, padding: 7 },
        { margin: 5, padding: 5 },
        { margin: 5, padding: 5 }
      ],
      colors: [['blue', '007ac3'], ['pink', 'e306a4'], ['teal', '30b0ab'], ['orange', 'f8991c'], ['turquoise', '1cdce7'], ['green', '40ae49'], ['hot-pink', 'e45296'], ['purple', '41016f']],
      answers: []
    }
  },
  computed: {
    outerClass () {
      return (this.quizInProgress || this.quizOver) ? `quiz` : `intro`
    }
  },
  methods: {
    startQuiz () {
      this.quizInProgress = true;
    },
    getJobsForGroup (groupNumber) {
      return this.jobs.filter(job => job.group == groupNumber)
    },
    yesOnClick () {
      this.answers.push(true)
      this.nextQuestion()
    },
    noOnClick () {
      this.answers.push(false)
      this.nextQuestion()
    },
    nextQuestion () {
      // check there are more questions
      if (this.questionNumber == this.questions.length - 1) this.finishQuiz()
      // check the upcoming question's prequesites are settled
      else {
        // update jobs
        this.updateJobStates()

        // next or finish
        let nextQuestion = this.questions[(this.questionNumber + 1)]
        this.questionNumber += 1
        // if the next question is invalid because of previous choices, skip it
        if (nextQuestion.prereqs !== undefined && !this.answers[nextQuestion.prereqs]) {
          this.answers.push(undefined)
          this.nextQuestion()
        }
      }
    },
    updateJobStates () {
      this.jobs.forEach(job => {
        for(let i=0; i<this.questionNumber+1; i++) { 
          // prereqs are [true, false] - 0 in either field means that condition nullifies the job IF it doesn't match the selection
          if (job.prereqs[i][0] == 0 && this.answers[i] == true) job.enabled = false
          else if (job.prereqs[i][1] == 0 && this.answers[i] == false) job.enabled = false
        }
      })
    },
    resetJobStates () {
      this.jobs.forEach(job => job.enabled = true)
    },
    finishQuiz () {
      this.quizOver = true
    }
  },
  created () {
    // load the jobs
    axios.get('/data.json')
      .then(res => {
        this.jobs = res.data 
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$width: 800px;
$height: 500px;

$yes: #f8981c;
$no: #bf0571;
$border: #0093d0;
$question-color: #35478a;
$question-font-size: 25pt;

#mental-health-questionnaire {
  display: block;
  width: $width;
  height: $height;

  * {
    margin: 0;
    padding: 0;
    font-size: 10px;
    line-height: 1.2em;
  }

  &.intro {
    background: url('../assets/intro-header.png') top center no-repeat;
  }
  &.quiz {
    background: url('../assets/quiz-background.jpg') center center no-repeat;
  }

  .lg { 
    font-size: 14pt; 
    margin: 20px;
  }
  .bold { font-weight: 700; }
  .gray { color: #6c6d70; }

  .intro {
    padding: 200px 30px 0;
    text-align: center;
    cursor: pointer;
  }

  .inner {
    display: flex;

    .left, .right {
      width: 180px;
    }
    .left {
      text-align: left;
    }
    .right {
      text-align: right;
    }
    .mid {
      flex: 1;

      .question-wrapper {
        height: 250px;
        border: 3px solid $border;
        background: white;
        border-radius: .5em;
        margin: 60px 30px 30px;
        padding: 10px;

        .question-number {
          font-size: 13pt;
          font-weight: 700;
          color: $border;
        }
        .question-content {
          margin-top: 20px;
          text-align: center;
          
          .icon {
            width: 100px;
            height: 100px;
            margin: 0 auto;
            text-align: center;
            img {
              position: relative;
              top: 50%;
              transform: translateY(-50%);
            }
          }

          .question-text-wrapper {
            display:table;
            height: 130px;
            .question-text {
              display: table-cell;
              vertical-align: middle;
              
              .question-title {
                font-size: 18pt;
                font-weight: 700;
                color: $question-color;
              }
              .question-sub-title {
                font-size: 10pt;
                color: $question-color;
                margin-top: 10px;
              }
            }
          }
        }
      }

      .question-answer-options {
        margin: 0 30px;
        button {
          cursor: pointer;
          width: calc(50% - 20px);
          height: 80px;
          font-size: 20pt;
          font-weight: 500;
          color: white;
          border: none;
          border-radius: 0.5em;
          &.yes {
            margin-right: 20px;
            background: $yes;
          }
          &.no {
            margin-left: 20px;
            background: $no;
          }
        }
      }

      .thank-you-wrapper {
        height: 290px;
        border: 3px solid $border;
        background: white;
        border-radius: .5em;
        margin: 60px 30px 30px;
        padding: 60px 10px 0;
        text-align: center;
      }
    }
  }
}
</style>
