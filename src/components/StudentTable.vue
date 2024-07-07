<script>
import StudentBody from '@/components/StudentBody.vue'
import axios from 'axios'
import { store } from '@/components/store.js'
import CorrectionTable from '@/components/CorrectionTable.vue'

export default {
  emits: ['keep-only-one-student', 'revert-student-information'],
  props: {
    studentInformation: {
      type: Array,
      required: true
    },
    correctionRef: {
      type: String,
      required: true
    }
  },
  components: {
    StudentBody,
    CorrectionTable
  },
  data() {
    return {
      isCorrecting: false,
      studentCorrectionProgress: [],
      studentCorrectionProgressBak: [],
      studentInformationTemp: []
    }
  },
  async mounted() {
    await this.getStudentProgress()
  },
  computed: {
    convertGender() {
      return (gender) => {
        return gender === '1' ? '男' : '女'
      }
    },
    defineStatus() {
      return (progress) => {
        return progress === 100 ? '已完成' : progress === 0 ? '未校正' : '校正中'
      }
    }
  },
  methods: {
    async getStudentProgress() {
      this.studentCorrectionProgress = []
      for (let studentInfo of this.studentInformation) {
        await axios.post(store.apiBaseURL + '/get_correction_progress', {
          studentKey: `${studentInfo['schoolName']}_${studentInfo['grade']}_${studentInfo['studentClass']}_${studentInfo['seatNumber']}_${studentInfo['studentName']}_${studentInfo['birthdayYear']}_${studentInfo['birthdayMonth']}_${studentInfo['birthdayDay']}_${studentInfo['gender']}`
        })
          .then(response => {
            this.studentCorrectionProgress.push(response.data['progress'])
          })
      }

    },
    keepOnlySelectedStudent(index) {
      this.studentCorrectionProgressBak = this.studentCorrectionProgress
      this.studentCorrectionProgress = [this.studentCorrectionProgressBak[index]]
      this.$emit('keep-only-one-student', index)
      this.isCorrecting = true
    },
    revertFilteredStudent(index) {
      this.studentCorrectionProgressBak[index] = this.studentCorrectionProgress[0]
      this.studentCorrectionProgress = this.studentCorrectionProgressBak
      this.$emit('revert-student-information')
      this.isCorrecting = false
    }
  }
}
</script>

<template>
  <div class="table-responsive" ref="correctionTable">
    <table class="table align-items-center table-flush " id="mainTable">
      <thead class="thead-light">
      <tr>
        <th scope="col" style="font-size: 18px;">學生姓名</th>
        <th scope="col" style="font-size: 18px;">年級班級座號</th>
        <th scope="col" style="font-size: 18px;">性別</th>
        <th scope="col" style="font-size: 18px;">狀態</th>
        <th scope="col" style="font-size: 18px;">完成度</th>
        <th scope="col" style="font-size: 18px;">校正按鈕</th>
      </tr>
      </thead>
      <StudentBody
        v-for="(information, index) in this.studentInformation"
        :key="information"
        :student-name="`${information['studentName']}`"
        :class-info="`${information['grade']}年 ${information['studentClass']}班 ${information['seatNumber']}號`"
        :gender="convertGender(information['gender'])"
        :correction-status="defineStatus(studentCorrectionProgress[index])"
        :progress="studentCorrectionProgress[index]"
        @keep-only-me="keepOnlySelectedStudent(index)"
        @back-to-student-info="revertFilteredStudent"
      />


    </table>
    <CorrectionTable
      v-if="isCorrecting"
      :student-information="studentInformation[0]"
      :correction-ref="correctionRef"
    />
  </div>
</template>

<style>
.drag-up-enter-active {
  transition: transform 0.6s, opacity 0.3s;
}

.drag-up-leave-active {
  transition: transform 0.6s, opacity 0.3s;
}

.drag-up-enter-from, .drag-up-leave-to {
  transform: translateY(-100%);
  opacity: 0;
}

.drag-up-leave {
  transform: translateY(0);
  opacity: 1;
}

@keyframes progress-bar-stripes {
  from {
    background-position: 1rem 0;
  }

  to {
    background-position: 0 0;
  }
}

@media screen and (prefers-reduced-motion: no-preference) {
}


.table-responsive {
  display: block;
  overflow-x: auto;
  width: 80vw;
  align-self: center;
  margin-top: 50px;
  transition: 0.5s;
}


.align-items-center {
  align-items: center;
}

table {
  border-collapse: collapse;
}

.table {
  width: 100%;
  margin-bottom: 1rem;
  background-color: transparent;
}

.table th,
.table td {
  /*padding: 1rem;*/
  vertical-align: top;
  /*border-top: 1px solid #e9ecef;*/
  border-radius: 10px 30px 10px 30px;
  min-width: max-content;
}

.table thead th {
  vertical-align: bottom;
  /*border-bottom: 5px solid rgba(255, 255, 255, 0);*/

}

.table .table {
  background-color: #f8f9fe;
}

.table-dark,
.table-dark > th,
.table-dark > td {
  background-color: #c1c2c3;
}

.table .thead-dark th {
  color: #f8f9fe;
  border-color: #1f3a68;
  background-color: #172b4d;
}

.table .thead-light th {
  color: #8898aa;
  border-color: #e9ecef;
  background-color: #f6f9fc;
}

.table-dark {
  color: #f8f9fe;
  background-color: #172b4d;
}

.table-dark th,
.table-dark td,
.table-dark thead th {
  border-color: #1f3a68;
}


.table thead th {
  /*font-size: .65rem;*/
  padding-top: .75rem;
  padding-bottom: .75rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  /*border-bottom: 1px solid #e9ecef;*/
}

.table th {
  font-weight: 600;
}

.table td,
.table th {
  /*font-size: .8125rem;*/
  white-space: nowrap;
}

.table.align-items-center td,
.table.align-items-center th {
  vertical-align: middle;
}

.table .thead-dark th {
  color: #4d7bca;
  background-color: #1c345d;
}

.table .thead-light th {
  color: #8898aa;
  background-color: #f6f9fc;
}

.table-flush td,
.table-flush th {
  border-right: 0;
  border-left: 0;
}

.table-flush tbody tr:first-child td,
.table-flush tbody tr:first-child th {
  border-top: 0;
}

.table-flush tbody tr:last-child td,
.table-flush tbody tr:last-child th {
  border-bottom: 0;
}

.card .table {
  margin-bottom: 0;
}

.card .table td,
.card .table th {
  padding-right: 1.5rem;
  padding-left: 1.5rem;
}

.btn-sm {
  font-size: .875rem;
  line-height: 1.5;
  padding: .25rem .5rem;
  border-radius: .375rem;
}

.btn .badge {
  position: relative;
  top: -1px;
}

.btn .badge:not(:first-child) {
  margin-left: .5rem;
}

.btn .badge:not(:last-child) {
  margin-right: .5rem;
}

.btn i:not(:first-child) {
  margin-left: .5rem;
}

.btn i:not(:last-child) {
  margin-right: .5rem;
}

.btn-sm {
  font-size: .75rem;
}

[class*='btn-outline-'] {
  border-width: 1px;
}

.btn-icon-only {
  width: 2.375rem;
  height: 2.375rem;
  padding: 0;
}

a.btn-icon-only {
  line-height: 2.5;
}

.btn-icon-only.btn-sm {
  width: 2rem;
  height: 2rem;
}

progress {
  vertical-align: baseline;
}

@media screen and (prefers-reduced-motion: reduce) {
}

.progress .sr-only {
  font-size: 13px;
  line-height: 20px;
  left: 0;
  /*clip: auto;*/
  /*width: auto;*/
  /*height: 20px;*/
  margin: 0 0 0 30px;
}


.bg-success {
  background-color: #2dce89 !important;
}

</style>