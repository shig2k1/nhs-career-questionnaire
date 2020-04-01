<template lang="pug">
  div
    #mental-health-questionnaire(:class="outerClass")
      .inner
        .left
          mental-health-jobs(v-for="i in 3" :key="`q-${i - 1}`" :color="colors[i - 1]" :jobs="getJobsForGroup(i - 1)" :group="groups[i - 1]")
        .mid
          // questions
          .questions(v-if="quizInProgress")
            .question-wrapper
              .question-number Question {{ questionNumber + 1 }}:
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
          .thank-you(v-if="quizOver")
            p Quiz over!
        .right
          mental-health-jobs(v-for="i in 5" :key="`q-${i + 2}`"  :color="colors[i + 2]" :jobs="getJobsForGroup(i + 2)" :group="groups[i + 2]")
    pre {{ answers }}
</template>

<script>
import MentalHealthJobs from './mental-health-jobs'

export default {
  name: 'mental-health-questionnaire',
  components: {
    MentalHealthJobs
  },
  data() {
    return {
      quizInProgress: true,
      quizOver: false,
      questionNumber: 0,
      questions: [
        { // 0
          headline: `Do you have an undergraduate degree or are you prepared to study at an undergraduate level?`,
          img: `q1-icon`
        },
        { // 1
          headline: `Do you have a medical degree or are you interested in undertaking a medical degree?`,
          prereqs: 0,
          img: `q1-icon`
        },
        { // 2
          headline: `Do you have a psychology degree`,
          img: `q3-icon`,
          prereqs: 0
        },
        { // 3
          headline: `Are you prepared to study at a post-graduate level?`,
          img: `q4-icon`,
          prereqs: 0
        },
        { // 4
          headline: `Are you working in a core profession* at the moment?`,
          subtitle: `*This includes, but is not limited to: applied psychology, nursing, medicine, and allied health professions.`,
          img: `q5-icon`,
          prereqs: 0
        },
        { // 5
          headline: `Do you have lived experience of mental health*? `,
          subtitle: `*Lived experience means you are living or have lived with mental illness, or you support or have supported someone living with mental illness.`,
          img: `q6-icon`
        },
        { // 6
          headline: `Are you interested in mostly working with adult patients?`,
          img: `q7-icon`
        },
        { // 7
          headline: `Do you have a background or experience in art, music or drama?`,
          img: `q8-icon`,
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
      jobs: [
        { // 0
          group: 0,
          name: `Mental Health Nurse`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 1
          group: 0,
          name: `Nursing associate`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 2
          group: 1,
          name: `Mental Health Support Worker`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 3
          group: 1,
          name: `Support, Time and Recovery Worker`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 4
          group: 1,
          name: `Assistant Practitioner`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 5
          group: 1,
          name: `Assistant psychologist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 0], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 6
          group: 1,
          name: `OT Assistant`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 7
          group: 1,
          name: `Peer Support Worker`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 0], [1, 1]],
          enabled: true
        },
        { // 8
          group: 2,
          name: `Psychological Wellbeing Practitioner`,
          url: ``,
          prereqs: [[1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 9
          group: 2,
          name: `Education Mental Health Practitioner`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [0, 1], [1, 1]],
          enabled: true
        },
        { // 10
          group: 2,
          name: `Child and Young People Wellbeing Practitioner`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [0, 1], [1, 1]],
          enabled: true
        },
        { // 11
          group: 3,
          name: `Psychotherapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 12
          group: 3,
          name: `Counsellor`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 13
          group: 3,
          name: `Child and Adolescent Psychotherapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 0], [1, 1], [1, 1], [0, 1], [1, 1]],
          enabled: true
        },
        { // 14
          group: 4,
          name: `Psychiatrist`,
          url: ``,
          prereqs: [[1, 0], [1, 0], [1, 1], [1, 0], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 15
          group: 5,
          name: `Social Worker`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 16
          group: 5,
          name: `Occupational Therapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 17
          group: 6,
          name: `Clinical Psychologist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 18
          group: 6,
          name: `Counselling Psychologist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 0], [1, 0], [1, 1], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 19
          group: 6,
          name: `High Intensity Therapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 1], [1, 0], [1, 1], [1, 0], [1, 1]],
          enabled: true
        },
        { // 20
          group: 7,
          name: `Music Therapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 0], [1, 1], [1, 1], [1, 0], [1, 0]],
          enabled: true
        },
        { // 20
          group: 7,
          name: `Art Therapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 0], [1, 1], [1, 1], [1, 0], [1, 0]],
          enabled: true
        },
        { // 20
          group: 7,
          name: `Drama Therapist`,
          url: ``,
          prereqs: [[1, 0], [1, 1], [1, 1], [1, 0], [1, 1], [1, 1], [1, 0], [1, 0]],
          enabled: true
        },
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
    startNewQuiz () {

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
        // if the next question is invalid because of previous choices, skip itthis.questionNumber += 1
        if (nextQuestion.prereqs !== undefined && !this.answers[nextQuestion.prereqs]) {
          this.answers.push(undefined)
          this.nextQuestion()
        }
      }
    },
    updateJobStates () {
      this.jobs.forEach(job => {
        for(let i=0; i< this.questionNumber; i++) { 
          console.log(job)
        }
      })
    },
    resetJobStates () {
      this.jobs.forEach(job => job.enabled = true)
    },
    finishQuiz () {
      this.quizInProgress = false
      this.quizOver = true
    }
  },
  created () {

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

html, body, * {
  margin: 0;
  padding: 0;
  font-size: 10px;
  line-height: 1.2em;
}

#mental-health-questionnaire {
  display: block;
  width: $width;
  height: $height;
  &.intro {
    background: url('../assets/intro-header.png') top center no-repeat;
  }
  &.quiz {
    background: url('../assets/quiz-background.jpg') center center no-repeat;
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
        // transform: translateY(-50%);
        .question-number {
          font-size: 13pt;
          font-weight: 700;
          color: $border;
        }
        .question-content {
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
    }
  }
}
</style>
