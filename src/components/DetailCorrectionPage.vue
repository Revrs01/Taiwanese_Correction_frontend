<script>
export default {
  props: {
    question: {
      type: String,
      default: "None"
    },
    whichSyllable: {
      type: Number,
      default: 1
    },
    audioPronunciation: {
      type: String
    },
    audioSentenceCreation: {
      type: String
    }
  },
  data() {
    return {
      openUserInputField: false,
      buttonList: {
        '1': '1 入聲消失',
        '2': '2 入聲念成 p',
        '3': '3 入聲念成 t',
        '4': '4 入聲念成 k',
        '5': '5 入聲念成 h',
        '6': '6 鼻音 ing 念成 in',
        '7': '7 鼻音 nn 不見',
        '8': '8 沒有鼻音念成有鼻音 nn',
        '9': '9 濁音 g 不見',
        '10': '10 濁音g唸成 k',
        '11': '11 濁音 b 不見',
        '12': '12 濁音 b 念成 m',
        '13': '13 濁音 l 念成 n',
        '14': '14 念成華語',
        '15': '15 念成台語的其他詞彙',
        '16': '16 其他錯誤'
      }
    }
  },
  methods: {
    updateCorrection(buttonIndex) {
      this.openUserInputField = buttonIndex === '16'
    }
  }
}
</script>

<template>
  <div class="modal" style="display: block" ref="modalBody">
    <div class="modal-content">
      <div class="group-title">{{ question }} 第{{ whichSyllable }}音節</div>
      <div class="audio-group">
        <div class="audio-title">唸詞</div>
        <audio controls style="display: block; margin: 0 auto 10px; width: 100%;">
          <source src='data:audio/wav;base64,{{ audioPronunciation }}' type="audio/wav">
        </audio>
        <div class="audio-title">造句</div>
        <audio controls style="display: block; margin: 0 auto auto; width: 100%;">
          <source src='data:audio/wav;base64,{{ audioSentenceCreation }}' type="audio/wav">
        </audio>
      </div>
      <div class="group-title" style="margin-top: 30px">快速校正按鈕</div>
      <div class="button-group">
        <button
          v-for="index in 16"
          :key="index"
          class="btn correction-btn"
          @click="updateCorrection(index.toString())"
        >{{ buttonList[index.toString()] }}
        </button>
      </div>

      <input
        v-if="openUserInputField"
        class="input-text-block"
        placeholder="請輸入其他錯誤描述">
      <button class="save-button" @click="() => {this.$refs.modalBody.style.display='none';}">儲存變更</button>

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
  display: none; /* Hidden by default */
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
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
  cursor: pointer;
}
</style>