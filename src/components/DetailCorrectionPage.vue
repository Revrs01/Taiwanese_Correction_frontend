<script>
import axios from 'axios'
import { store } from '@/components/store.js'
import { nextTick } from 'vue'

export default {
  emits: ['update-syllable-correction', 'close-without-save'],
  props: {
    question: {
      type: String
    },
    questionOrder: {
      type: String
    },
    whichSyllable: {
      type: Number
    },
    audioPronunciation: {
      type: String
    },
    audioSentenceCreation: {
      type: String
    },
    oldCorrectionValue: {
      type: [Array, String, undefined]
    }
  },
  async mounted() {
    await this.getDetailCorrectionButton()
    if (this.oldCorrectionValue === undefined) {
      console.log('oldValue is undefined')
      return null
    } else if (typeof this.oldCorrectionValue === "string") {
      this.currentPickedButtons = "21"
      this.openUserInputField = true
      await nextTick()
      this.$refs.inputField.value = this.oldCorrectionValue
    } else {
      this.currentPickedButtons = new Set(this.oldCorrectionValue)
    }
  },
  data() {
    return {
      activeCorrectionButton: null,
      openUserInputField: false,
      currentPickedButtons: new Set(),
      isPronounAudioMounted: false,
      isCreationAudioMounted: false,
      buttonList: {
        '1': '1 入聲消失',
        '2': '2 入聲念成 p',
        '3': '3 入聲念成 t',
        '4': '4 入聲念成 k',
        '5': '5 入聲念成 h',
        '6': '6 鼻音 ing 念成 in',
        '7': '7 鼻音 nn 不見',
        '8': '8 鼻音 nn 唸成 n',
        '9': '9 鼻音 m 唸成 n',
        '10': '10 濁音 g 不見',
        '11': '11 濁音g唸成k',
        '12': '12 濁音b不見',
        '13': '13 濁音b念成m (m念成b)',
        '14': '14 濁音l念成n',
        '15': '15 受華語影響（聲母）',
        '16': '16 受華語影響（韻母）',
        '17': '17 受華語影響（聲母+韻母）',
        '18': '18 受華語影響（聲調）',
        '19': '19 華語讀音',
        '20': '20 念成台語的其他詞彙',
        '21': '21 其他錯誤（手動輸入）',
        '22': '22 音節正確'
      }
    }
  },
  methods: {
    async getDetailCorrectionButton() {
      await axios.post(store.apiBaseURL + '/get_detail_correction_button', {
        correctionRef: '2024_07',
        questionNumber: this.questionOrder,
        whichSyllable: this.whichSyllable
      })
        .then(response => {
          if (response.status === 702) {
            alert('questionNumber or syllable doesn\'t exist !')
            alert(`questionNumber=${this.questionOrder}, whichSyllable=${this.whichSyllable}`)
            return
          }
          this.activeCorrectionButton = response.data
        })
    },
    buttonClickHandler(buttonIndex) {
      if (buttonIndex === '21') {
        // if I click button "21"
        if (this.currentPickedButtons !== "21") {
          // if I don't activate "21", button "21" needs to be the only pick
          this.currentPickedButtons = '21'
          this.openUserInputField = true
        } else {
          // if "21" is activated, remove everything I picked then deactivate "21"
          this.openUserInputField = false
          this.currentPickedButtons = new Set()
        }
        return
      } else if (this.currentPickedButtons === "21") {
        // if other buttons are clicked, clear everything then close input field
        this.currentPickedButtons = new Set()
        this.openUserInputField = false
      }

      if (this.currentPickedButtons.has(buttonIndex)) {
        this.currentPickedButtons.delete(buttonIndex)
      } else {
        this.currentPickedButtons.add(buttonIndex)
      }
    },
    saveChangedCorrection() {
      if (this.currentPickedButtons === new Set(this.oldCorrectionValue)) {
        this.closeDetailCorrectionWithoutSaving()
        console.log('button doesn\'t changed, close without saving')
        return
      } else if (this.currentPickedButtons === this.oldCorrectionValue) {
        this.closeDetailCorrectionWithoutSaving()
        console.log('text doesn\'t changed, close without saving')
      }

      let emitObject = {
        questionOrder: this.questionOrder,
        whichSyllable: this.whichSyllable,
        returnValue: null
      }

      if (typeof this.currentPickedButtons === "string") {
        const inputFieldValue = this.$refs.inputField.value
        // console.log(intInputFieldValue)
        if (/^[+-]?\d+(\.\d+)?([eE][+-]?\d+)?$/.test(inputFieldValue)) {
          alert('請不要在輸入框只輸入數字')
          return
        } else {
          emitObject['returnValue'] = inputFieldValue
          this.$emit('update-syllable-correction', emitObject)
          return
        }
      }

      emitObject['returnValue'] = Array.from(this.currentPickedButtons)
      this.$emit('update-syllable-correction', emitObject)
    },
    closeDetailCorrectionWithoutSaving() {
      this.$emit('close-without-save')
    }
  },
  computed: {
    getButtonClass() {
      return (buttonIndex) => {
        if (this.currentPickedButtons === null) {
          return null
        }

        if (typeof this.currentPickedButtons === "string") {
          if (buttonIndex === "21")
            return 'btn-primary'
        } else {
          if (this.currentPickedButtons.has(buttonIndex)) {
            return 'btn-primary'
          }
        }


      }
    },
    isButtonDisabled() {
      return (buttonIndex) => {
        return !this.activeCorrectionButton || !this.activeCorrectionButton.includes(buttonIndex)
      }
    }
  }
}
</script>

<template>
  <div class="modal" style="display: block" ref="modalBody">
    <div class="modal-content">
      <button class="close" @click="closeDetailCorrectionWithoutSaving">&times;</button>
      <div class="group-title">{{ question }} 第{{ whichSyllable }}音節</div>
      <div class="audio-group">
        <div class="audio-title">唸詞</div>
        <audio controls style="display: block; margin: 0 auto 10px; width: 100%;">
          <source :src="`data:audio/wav;base64,${audioPronunciation}`" type="audio/wav">
        </audio>
        <div class="audio-title">造句</div>
        <audio controls style="display: block; margin: 0 auto auto; width: 100%;">
          <source :src='`data:audio/wav;base64,${audioSentenceCreation}`' type="audio/wav">
        </audio>
      </div>
      <div class="group-title" style="margin-top: 30px">快速校正按鈕</div>
      <div class="button-group">
        <button
          v-for="index in Object.keys(buttonList).length"
          :key="index"
          class="btn correction-btn"
          :class="getButtonClass(index.toString())"
          :disabled="isButtonDisabled(index.toString())"
          @click="buttonClickHandler(index.toString())"
        >{{ buttonList[index.toString()] }}
        </button>
      </div>

      <input
        v-if="openUserInputField"
        class="input-text-block"
        ref="inputField"
        placeholder="請輸入其他錯誤描述">
      <button class="save-button" @click="saveChangedCorrection">儲存變更</button>

    </div>

  </div>
</template>

<style scoped>

.save-button {
  height: 50px;
  width: 100%;
  border-radius: 15px;
  background-color: #21b16e;
  font-size: 28px;
  color: white;
  border: none;
  margin-top: 10px;
}

.save-button:hover {
  background-color: #1d9a60;
  cursor: pointer;
}

.input-text-block {
  width: 100%;
  height: 50px;
  border-radius: 10px;
  padding: 10px;
  border: solid gray;
  margin-top: 10px;
}

.group-title {
  font-size: 28px;
  color: black;
}

.audio-title {
  font-size: 20px;
  color: black;
}

.button-group, .audio-group {
  border: solid black;
  border-radius: 10px;
  display: flex;
  flex-wrap: wrap;
  padding: 10px;
}

.correction-btn {
  margin: 5px 5px;
}

.modal {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content */
.modal-content {
  border-radius: 10px;
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}

/* The Close Button */
.close {
  color: #aaaaaa;
  background-color: inherit;
  float: right;
  font-size: 40px;
  margin-top: -10px;
  font-weight: bold;
  border: none;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}

.btn-primary {
  color: white;
  background-color: #0d6efd;
}
</style>