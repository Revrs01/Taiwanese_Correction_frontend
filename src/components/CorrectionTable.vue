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
    }
  },
  components: { CorrectionBody },
  data() {
    return {

      questionOrder: [],
      questionList: [],
      correctionDetails: [],
      currentStudentKey: ''
    }
  },
  async mounted() {
    // this.isMounted = true
    await this.getCorrectionData()
    await this.getCorrectionDetails(`${this.studentInformation['schoolName']}_${this.studentInformation['grade']}_${this.studentInformation['studentClass']}_${this.studentInformation['seatNumber']}_${this.studentInformation['studentName']}_${this.studentInformation['birthdayYear']}_${this.studentInformation['birthdayMonth']}_${this.studentInformation['birthdayDay']}_${this.studentInformation['gender']}`)
  },
  methods: {
    async getCorrectionData() {
      await axios.post(store.apiBaseURL + '/fetch_test_question', {
        studentLevel: this.studentInformation['grade'] <= 2 ? 'low' : 'high'
      })
        .then(response => {
          this.questionOrder = response.data['_order']
          this.questionList = response.data['questionList']
          // this.correctionData = response.data
          // this.getCorrectionDetails()
        })
    },

    async getCorrectionDetails(studentKey) {
      const studentInfo = {
        studentKey: studentKey
      }
      console.log(studentInfo)

      await axios.post(store.apiBaseURL + '/get_correction_details', studentInfo)
        .then(response => {
          let temp = response.data
          this.correctionDetails = []
          for (let i of this.questionOrder) {
            this.correctionDetails.push(temp[i])
          }
          console.log(this.correctionDetails)
        })
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
      />

    </table>
  </div>
</template>

