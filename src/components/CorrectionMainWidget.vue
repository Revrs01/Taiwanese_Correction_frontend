<script>

import FilterWindow from '@/components/FilterWindow.vue'
import StudentTable from '@/components/StudentTable.vue'
import { store } from '@/components/store.js'
import axios from 'axios'

export default {
  emits: ["change-icon"],
  props: {
    shouldStopCorrection: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      studentInformation: [],
      studentInformationBak: [],
      isFilterWindowVisible: true,
      isStudentTableVisible: false,
      currentState: this.state,
      correctionRef: ''
    }
  },
  components: {
    FilterWindow,
    StudentTable
  },
  methods: {
    async getStudentInformation(options) {
      try {
        this.correctionRef = options.correctionRef
        await axios.post(store.apiBaseURL + '/filter_by_options', options)
        .then((response) => {
          this.studentInformation = response.data
          this.correctionStart()
        })
      } catch (e) {
        alert(e)
      }
    },
    correctionStart() {
      this.isStudentTableVisible = !this.isStudentTableVisible
      this.isFilterWindowVisible = !this.isFilterWindowVisible
      this.$emit('change-icon')
    },
    correctionStop() {
      this.isStudentTableVisible = false
      this.isFilterWindowVisible = true
    },
    keepOnlyOneStudent(index) {
      this.studentInformationBak = this.studentInformation
      this.studentInformation = [this.studentInformationBak[index]]

    },
    revertStudentInformation() {
      this.studentInformation = this.studentInformationBak
      this.studentInformationBak = []
    }

  },
  watch: {
    shouldStopCorrection(newVal) {
      if (newVal) {
        this.correctionStop();
      }
    }
  }
}
</script>

<template>
  <FilterWindow @filter-student="getStudentInformation"
    v-if="isFilterWindowVisible"
  />
  <StudentTable
    v-if="isStudentTableVisible"
    :student-information="studentInformation"
    :correction-ref="correctionRef"
    @keep-only-one-student="keepOnlyOneStudent"
    @revert-student-information="revertStudentInformation"
  />
</template>

<style scoped>

</style>