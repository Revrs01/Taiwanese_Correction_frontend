<script>

import { defineComponent } from 'vue'
import CorrectionBody from '@/components/CorrectionBody.vue'
import axios from 'axios'
import { store } from '@/components/store.js'

export default defineComponent({
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
  components: { CorrectionBody },
  data() {
    return {
      studentLevel: `${this.studentInformation['schoolName']}_${this.studentInformation['grade']}_${this.studentInformation['studentClass']}_${this.studentInformation['seatNumber']}_${this.studentInformation['studentName']}_${this.studentInformation['birthdayYear']}_${this.studentInformation['birthdayMonth']}_${this.studentInformation['birthdayDay']}_${this.studentInformation['gender']}`,
      questionOrder: [],
      questionList: [],
      correctionDetails: [],
      currentStudentKey: '',
      buttonKeys: []
    }
  },
  async mounted() {
    // this.isMounted = true
    await this.getCorrectionData()
    await this.getCorrectionDetails(this.studentLevel, this.studentInformation['grade'] <= 2 ? 'low' : 'high')
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
          // this.correctionData = response.data
          // this.getCorrectionDetails()
        })
    },
    async getCorrectionDetails(studentKey, studentLevel) {
      const studentInfo = {
        studentKey: studentKey,
        studentLevel: studentLevel,
        correctionRef: this.correctionRef
      }
      // console.log(studentInfo)

      await axios.post(store.apiBaseURL + '/get_correction_details', studentInfo)
        .then(response => {
          let temp = response.data
          this.correctionDetails = []
          for (let i of this.questionOrder) {
            // correctionDetails will contain undefined if question is not stored in correctionTable
            // this won't cause any error I think ...
            this.correctionDetails.push(temp[i])
          }
          // console.log(this.correctionDetails)
        })
    },
    async updateAssessmentValue(updatedValue, questionIndex, index) {
      let studentId = `${this.studentInformation['schoolName']}_${this.studentInformation['grade']}_${this.studentInformation['studentClass']}_${this.studentInformation['seatNumber']}_${this.studentInformation['studentName']}_${this.studentInformation['birthdayYear']}_${this.studentInformation['birthdayMonth']}_${this.studentInformation['birthdayDay']}_${this.studentInformation['gender']}`
      // console.log(updatedValue, questionIndex)
      await axios.post(store.apiBaseURL + '/update_correction_details', {
        studentId: studentId,
        questionIndex: questionIndex,
        value: updatedValue
      })
        .then(async () => {
          await this.syncAssessmentValueUpdate(studentId, index, questionIndex)
        })
    },
    async syncAssessmentValueUpdate(studentId, index, questionIndex) {
      try {
        await axios.post(store.apiBaseURL + '/get_correction_details', {
          studentKey: studentId,
          studentLevel: this.studentLevel,
          correctionRef: this.correctionRef
        })
          .then(response => {
            this.correctionDetails[index] = response.data[questionIndex]
            console.log('Sync with DB')
          })
      } catch (e) {
        alert(e)
      }
    }
  }
})
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
        :assessment="correctionDetails[index]"
        :button-keys="buttonKeys[index]"
        @assessment-value-update="updateAssessmentValue($event, questionOrder[index], index)"
      />

    </table>
  </div>
</template>

