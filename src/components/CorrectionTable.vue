<script>

import CorrectionBody from '@/components/CorrectionBody.vue'
import DetailCorrectionPage from '@/components/DetailCorrectionPage.vue'
import axios from 'axios'
import { store } from '@/components/store.js'

export default {
  emits: ['update-progress'],
  props: {
    studentInformation: {
      type: Object,
      required: true
    },
    correctionRef: {
      type: String,
      required: true
    }
  },
  components: { DetailCorrectionPage, CorrectionBody },
  data() {
    return {
      studentId: `${this.studentInformation['schoolName']}_${this.studentInformation['grade']}_${this.studentInformation['studentClass']}_${this.studentInformation['seatNumber']}_${this.studentInformation['studentName']}_${this.studentInformation['birthdayYear']}_${this.studentInformation['birthdayMonth']}_${this.studentInformation['birthdayDay']}_${this.studentInformation['gender']}`,
      studentLevel: this.studentInformation['grade'] <= 2 ? 'low' : 'high',
      questionOrder: [],
      questionList: [],
      correctionDetails: [],
      currentStudentKey: '',
      buttonKeys: [],
      isCorrectingSyllable: false,

      // these are for 2nd correction, args will pass to DetailCorrectionPage
      currentCorrectingSyllable: null,
      currentCorrectingQuestion: null,
      currentCorrectingQuestionOrder: null,
      currentCorrectingQuestionIndex: null,
      oldSyllableCorrectionValue: null
    }
  },
  async mounted() {
    // this.isMounted = true
    await this.getCorrectionData()
    await this.getCorrectionDetails()
  },
  methods: {
    async getCorrectionData() {
      await axios.post(store.apiBaseURL + '/fetch_test_question', {
        studentLevel: this.studentInformation['grade'] <= 2 ? 'low' : 'high',
        correctionRef: this.correctionRef
      })
        .then(response => {
          this.questionOrder = response.data['_order']
          this.questionList = response.data['questionList']
          this.buttonKeys = response.data['buttonKeys']
        })
    },
    async getCorrectionDetails() {
      const studentInfo = {
        studentKey: this.studentId,
        studentLevel: this.studentLevel,
        correctionRef: this.correctionRef
      }

      await axios.post(store.apiBaseURL + '/get_correction_details', studentInfo)
        .then(response => {
          let temp = response.data
          if (response.status === 701) {
            alert('這位學生沒有第一次校正的結果')
            return
          }
          this.correctionDetails = []
          for (let i of this.questionOrder) {
            // correctionDetails will contain undefined if question is not stored in correctionTable
            // this won't cause any error I think ...
            this.correctionDetails.push(temp[i])
          }
        })
    },
    async updateAssessmentValue(updatedValue, questionIndex, index, syllable = null) {
      let updateObject = {
        studentId: this.studentId,
        questionNumber: questionIndex,
        newValue: updatedValue,
        correctionRef: this.correctionRef
      }
      if (syllable !== null) {
        updateObject['syllable'] = syllable
      }
      await axios.post(store.apiBaseURL + '/update_correction_details', updateObject)
        .then(async () => {
          await this.syncAssessmentValueUpdate(index, questionIndex)
        })
    },
    async syncAssessmentValueUpdate(index, questionIndex) {
      try {
        await axios.post(store.apiBaseURL + '/get_correction_details', {
          studentKey: this.studentId,
          studentLevel: this.studentLevel,
          correctionRef: this.correctionRef
        })
          .then(response => {
            this.correctionDetails[index] = response.data[questionIndex]
            this.calcCurrentProgress()
            console.log('Sync with DB')
          })
      } catch (e) {
        alert(e)
      }
    },
    calcCurrentProgress() {
      let currentProgress
      let cnt = 0
      if (this.correctionRef === '2023_02') {
        for (let x of this.correctionDetails) {
          if (x) {
            cnt += 1
          }
        }
        currentProgress = Math.floor(cnt * 100 / this.correctionDetails.length)
      } else if (this.correctionRef === '2024_07') {
        // console.log('progress for 2024_07 is not available yet !')
        for (let x of this.correctionDetails) {
          cnt += Object.keys(x).length
        }
        currentProgress = Math.floor(cnt * 100 / this.buttonKeys.reduce((accumulator, currentValue) => {
          return accumulator + currentValue
        }, 0))
      }

      this.$emit('update-progress', currentProgress)
    },
    startCorrectingSyllable(emitObject, questionOrder, question, index) {
      this.oldSyllableCorrectionValue = emitObject['oldValue']
      this.currentCorrectingQuestionOrder = questionOrder
      this.currentCorrectingQuestion = question
      this.currentCorrectingSyllable = emitObject['syllable']
      this.currentCorrectingQuestionIndex = index
      this.isCorrectingSyllable = true
    },
    endCorrectingSyllable(emitObject) {
      this.updateAssessmentValue(emitObject['returnValue'], emitObject['questionOrder'], this.currentCorrectingQuestionIndex, emitObject['whichSyllable'])
      this.isCorrectingSyllable = false
      this.currentCorrectingQuestion = null
      this.currentCorrectingSyllable = null
      this.currentCorrectingQuestionIndex = null
    },
    closeDetailCorrectionWithoutSaving() {
      this.isCorrectingSyllable = false
      this.currentCorrectingQuestion = null
      this.currentCorrectingSyllable = null
      this.currentCorrectingQuestionIndex = null
    }
  }
}
</script>

<template>
  <div class="table-responsive" ref="correctionTable">
    <table class="table align-items-center table-flush " id="mainTable">
      <thead class="thead-light">
      <tr>
        <th scope="col" style="font-size: 18px;">題目</th>
        <th scope="col" style="font-size: 18px;">錄音檔</th>
        <th scope="col" style="font-size: 18px;">評分</th>
      </tr>
      </thead>
      <CorrectionBody
        v-for="(question, index) in questionList"
        :key="index"
        :question="question"
        :question-order="questionOrder[index]"
        :assessment="correctionDetails[index]"
        :button-keys="buttonKeys[index]"
        @assessment-value-update="updateAssessmentValue($event, questionOrder[index], index)"
        @start-syllable-correction="startCorrectingSyllable($event, questionOrder[index], question, index)"
      />
      <DetailCorrectionPage
        v-if="isCorrectingSyllable"
        :question="currentCorrectingQuestion"
        :question-order="currentCorrectingQuestionOrder"
        :old-correction-value="oldSyllableCorrectionValue"
        :which-syllable="currentCorrectingSyllable"
        @update-syllable-correction="endCorrectingSyllable($event)"
        @close-without-save="closeDetailCorrectionWithoutSaving"
      />
    </table>
  </div>
</template>

