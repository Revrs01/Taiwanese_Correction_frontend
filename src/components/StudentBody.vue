<script>
export default {
  name: 'StudentBody',
  emits: ['keep-only-me', 'back-to-student-info'],
  props: {
    studentName: {
      type: String,
      required: true
    },
    classInfo: {
      type: String,
      required: true
    },
    gender: {
      type: String,
      required: true
    },
    correctionStatus: {
      type: String,
      required: true
    },
    progress: {
      type: Number,
      required: false
    }
  },
  data() {
    return {
      currentButtonTextIndex: 0
    }
  },
  methods: {
    startCorrection() {
      // buttonText: ["intoCorrection", "backToStudentInformation"]
      this.currentButtonTextIndex = 1
      this.$emit('keep-only-me')
    },
    exitCorrection() {
      this.currentButtonTextIndex = 0
      this.$emit('back-to-student-info')
    }
  }
}
</script>

<template>
  <tbody>
  <tr>
    <th scope="row" class="th-block">{{ studentName }}</th>
    <th scope="row" class="th-block">{{ classInfo }}</th>
    <th scope="row" class="th-block">{{ gender }}</th>
    <th class="th-block">{{ correctionStatus }}</th>
    <td class="th-block" style="max-width: 100px;">
      <div class="d-flex align-items-center justify-contents-center">
        <div>
          <span class="progress-percent">{{ progress }}%</span>
        </div>
        <div>
          <div class="progress" style="height: 15px;">
            <div class="progress-bar" role="progressbar" aria-valuenow="{{ progress }}"
                 aria-valuemin="0" aria-valuemax="100"
                 :style="{ width: progress + '%', backgroundColor: '#2dce89' }">
            </div>
          </div>
        </div>
      </div>
    </td>
    <th scope="row">
      <div class="d-flex">
        <button v-if="currentButtonTextIndex === 0" class="btn"
                style="color: white; background-color: orange; width: 100%;" @click="startCorrection">進入校正
        </button>
        <button v-if="currentButtonTextIndex === 1" class="btn"
                style="color: white; background-color: #ff6a00; width: 100%;" @click="exitCorrection">返回學生頁面
        </button>
      </div>
    </th>
  </tr>

  </tbody>
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

.progress-percent {
  margin-right: .5rem !important;
  min-width: 34px;
  text-align: end;
  color: black;
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
  .progress-bar {
    transition: 0.5s;
  }
}


.d-flex {
  display: flex !important;
}


.align-items-center {
  align-items: center;
}

.justify-contents-center {
  justify-content: center;
}

table {
  border-collapse: collapse;
}

.table th,
.table td {
  /*padding: 1rem;*/
  vertical-align: top;
  /*border-top: 1px solid #e9ecef;*/
  border-radius: 20px;
  min-width: max-content;
}

.table thead th {
  vertical-align: bottom;
  /*border-bottom: 2px solid #e9ecef;*/
}

.table tbody + tbody {
  border-top: 5px solid rgba(255, 255, 255, 0);
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

.table td .progress {
  width: 120px;
  height: 3px;
  margin: 0;
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

.card .table td,
.card .table th {
  padding-right: 1.5rem;
  padding-left: 1.5rem;
}

.th-block {
  font-size: 16px;
  background-color: rgba(216, 215, 215, 0.35);
  color: black;
  border-radius: 20px;
  overflow-x: hidden;
}

.btn {
  font-weight: 600;
  line-height: 1.5;
  display: inline-block;
  padding: .625rem 1.25rem;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  transition: color .15s ease-in-out, background-color .15s ease-in-out, border-color .15s ease-in-out, box-shadow .15s ease-in-out;
  text-align: center;
  vertical-align: middle;
  white-space: nowrap;
  border: 1px solid transparent;
  border-radius: 20px;
  /*width: 100%;*/
}

.btn:hover,
.btn:focus {
  text-decoration: none;
}

.btn:focus {
  outline: 0;
  box-shadow: 0 7px 14px rgba(50, 50, 93, .1), 0 3px 6px rgba(0, 0, 0, .08);
}

a:not(.btn) {
  text-decoration: underline;
}

.btn {
  font-size: 16px;
  position: relative;
  transition: all .15s ease;
  letter-spacing: .025em;
  text-transform: none;
  will-change: transform;
}

.btn:hover {
  transform: translateY(-1px);
  box-shadow: 0 7px 14px rgba(50, 50, 93, .1), 0 3px 6px rgba(0, 0, 0, .08);
}

.btn:not(:last-child) {
  margin-right: .5rem;
}

.btn i:not(:first-child) {
  margin-left: .5rem;
}

.btn i:not(:last-child) {
  margin-right: .5rem;
}

[class*='btn-outline-'] {
  border-width: 1px;
}

progress {
  vertical-align: baseline;
}

.progress-bar {
  display: flex;
  flex-direction: column;
  transition: width .6s ease;
  text-align: center;
  white-space: nowrap;
  color: #fff;
  background-color: #5e72e4;
  justify-content: center;
}

@media screen and (prefers-reduced-motion: reduce) {
  .progress-bar {
    transition: none;
  }
}

.progress {
  overflow: hidden;
  /*height: 15px;*/
  margin-bottom: 1rem;
  border-radius: .25rem;
  background-color: #d4dbdc;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, .1);
  /*justify-content: center;*/
  /*height: 20px;*/
}

.progress-bar {
  height: 100%;
  border-radius: 0;
  box-shadow: none;
  width: 100%;
}


</style>