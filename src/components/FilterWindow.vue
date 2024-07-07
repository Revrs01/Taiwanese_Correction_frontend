<script>
import axios from 'axios'
import { store } from '@/components/store.js'

export default {
  name: 'FilterWindow',
  emits: ['filter-student'],
  data() {
    return {
      distinctFilterSelection: {},
      selectedSchool: '',
      selectedGrade: '',
      selectedClass: '',
      correctionRef: ''
    }
  },
  mounted() {
    this.getFilterSelection()
  },
  methods: {
    async getFilterSelection() {
      try {
        await axios.get(store.apiBaseURL + '/fetch_filter_selection')
          .then(response => {
            this.distinctFilterSelection = response.data
          })
      } catch (e) {
        alert(e)
      }

    },
    filterStudent() {
      if (!this.selectedSchool && !this.selectedGrade && !this.selectedClass) {
        alert('請至少選擇一個進行篩選')
        return
      }
      if (!this.correctionRef) {
        alert('請選擇校正表單')
        return
      }
      let options = {
        options: {
          schoolName: this.selectedSchool,
          studentClass: this.selectedClass,
          grade: this.selectedGrade,
          correctionRef: this.correctionRef
        }
      }
      this.$emit('filter-student', options)
    }
  }
}
</script>

<template>
  <div class="window-block flex-col" ref="filterWindowRoot">
    <div class="col input-block">
      <div class="filter-title">學校名稱</div>
      <select class="select-block padding-start" v-model="selectedSchool">
        <option value="" selected>選擇學校</option>
        <option v-for="(schoolName, index) in distinctFilterSelection['distinctSchoolName']"
                :value="schoolName"
                :key="index"
        >
          {{ schoolName }}
        </option>
      </select>
    </div>
    <div class="col input-block" style="padding: 1px 1px 0 1px">
      <div class="row padding-mid-lane">
        <div class="filter-title align-start input-block">年級</div>
        <div class="filter-title align-mid input-block">班級</div>
      </div>

      <div class="row">
        <select class="select-block shorten-width align-start padding-start" v-model="selectedGrade">
          <option value="" selected>選擇年級</option>
          <option v-for="(grade, index) in distinctFilterSelection['distinctGrade']"
                  :value="grade"
                  :key="index"
          >
            {{ grade }}
          </option>
        </select>

        <select class="select-block shorten-width padding-start" v-model="selectedClass">
          <option value="" selected>選擇班級</option>
          <option v-for="(cls, index) in distinctFilterSelection['distinctStudentClass']"
                  :value="cls"
                  :key="index"
          >
            {{ cls }}
          </option>
        </select>
      </div>
    </div>
    <div class="row">
      <select class="select-block padding-start" v-model="correctionRef">
        <option value="" selected>選擇校正表單</option>
        <option value="2023_02">全部學生</option>
        <option value="2024_07">第二次校正</option>
      </select>
    </div>
    <div class="row">
      <button type="button" class="select-block shorten-button-size button-color"
              @click="filterStudent">送出篩選
      </button>
    </div>
  </div>
</template>

<style scoped>

.flex-col {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.window-block {
  transition: 0.5s;
  width: 70vw;
  height: 50vh;
  display: block;
  background-color: lightgrey;
  align-self: center;
  border-radius: 20px;
  border: solid 10px;
  min-width: 400px;
  min-height: 300px;
}

.select-block {
  height: 40px;
  width: 100%;
  border-radius: 25px;
  font-size: 16px;
}

.shorten-width {
  width: 48%;
}

.align-start {
  flex: 0 0 auto;
}

.padding-start {
  padding: 10px;
}

.padding-mid-lane {
  padding: 10px 10px 0 0;
}

.align-mid {
  flex: 1;
  text-align: center;
}

.shorten-button-size {
  width: 98%;
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

.col {
  display: flex;
  flex-direction: column;
}

.filter-title {
  font-size: 28px;
  color: black;
}

.input-block {
  padding: 10px 8px 0 8px;
}

.button-color {
  background-color: #349e61;
  color: white;
  border: solid lightgray;
}

.button-color:hover {
  background-color: seagreen;
  cursor: pointer;
}
</style>