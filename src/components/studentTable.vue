<script>
export default {
  methods: {
    navOpen() {
      this.$refs.correctionTable.classList.toggle('push-right')
    },
    navClose() {
      this.$refs.correctionTable.classList.toggle('push-right')
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
        <!--        <th scope="col" style="font-size: 18px;">年級</th>-->
        <!--        <th scope="col" style="font-size: 18px;">班級</th>-->
        <!--        <th scope="col" style="font-size: 18px;">座號</th>-->
        <th scope="col" style="font-size: 18px;">性別</th>
        <th scope="col" style="font-size: 18px;">狀態</th>
        <th scope="col" style="font-size: 18px;">完成度</th>
        <th scope="col" style="font-size: 18px;">校正按鈕</th>
      </tr>
      </thead>
      <tbody>
      <tr>
        <th scope="row" class="th-block">搭拉崩吧</th>
        <th scope="row" class="th-block">6年5班 20號</th>
        <th scope="row" class="th-block">女</th>
        <th class="th-block">
          校正中
        </th>
        <td class="th-block" style="max-width: 100px;">
          <div class="d-flex align-items-center justify-contents-center">
            <div>
              <span class="progress-percent">69%</span>
            </div>
            <div>
              <div class="progress" style="height: 15px;">
                <div class="progress-bar" role="progressbar" aria-valuenow="69"
                     aria-valuemin="0" aria-valuemax="100" style="width: 69%; background-color: #2dce89"></div>
              </div>
            </div>
          </div>
        </td>
        <th scope="row">
          <div class="d-flex">
            <button class="btn" style="color: white; background-color: orange;" buttonIndex="${index}">進入校正
            </button>
          </div>
        </th>
      </tr>

      </tbody>

    </table>
  </div>
</template>

<style scoped>
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

.justify-contents-center {
  justify-content: center;
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
  border-radius: 20px;
  min-width: max-content;
}

.table thead th {
  vertical-align: bottom;
  /*border-bottom: 2px solid #e9ecef;*/
}

.table tbody + tbody {
  border-top: 2px solid #e9ecef;
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

.card .table {
  margin-bottom: 0;
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
  width: 100%;
}

.btn:hover,
.btn:focus {
  text-decoration: none;
}

.btn:focus {
  outline: 0;
  box-shadow: 0 7px 14px rgba(50, 50, 93, .1), 0 3px 6px rgba(0, 0, 0, .08);
}

.btn.disabled,
.btn:disabled {
  opacity: .65;
  box-shadow: none;
}

.btn:not(:disabled):not(.disabled) {
  cursor: pointer;
}

.btn:not(:disabled):not(.disabled):active,
.btn:not(:disabled):not(.disabled).active {
  box-shadow: none;
}

.btn:not(:disabled):not(.disabled):active:focus,
.btn:not(:disabled):not(.disabled).active:focus {
  box-shadow: 0 7px 14px rgba(50, 50, 93, .1), 0 3px 6px rgba(0, 0, 0, .08);
}

a.btn.disabled {
  pointer-events: none;
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

a:not(.btn) {
  text-decoration: underline;
}

.btn .badge:not(:first-child) {
  margin-left: .5rem;
}

.btn .badge:not(:last-child) {
  margin-right: .5rem;
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

.progress .sr-only {
  font-size: 13px;
  line-height: 20px;
  left: 0;
  /*clip: auto;*/
  /*width: auto;*/
  /*height: 20px;*/
  margin: 0 0 0 30px;
}

.progress-bar {
  height: 100%;
  border-radius: 0;
  box-shadow: none;
  width: 100%;
}


.bg-success {
  background-color: #2dce89 !important;
}

</style>