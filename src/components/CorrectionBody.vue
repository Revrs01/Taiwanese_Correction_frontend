<script>
export default {
  emits: ['assessment-value-update', 'start-syllable-correction'],
  props: {
    question: {
      type: String,
      required: true
    },
    questionOrder: {
      type: String,
      required: true
    },
    audio: {
      type: String
    },
    assessment: {
      type: [String, Object]
    },
    buttonKeys: {
      type: [Array, Number]
    }
  },
  data() {
    return {}
  },
  computed: {
    getButtonClass() {
      return (buttonKey) => {
        if (this.assessment === undefined) {
          return 'btn-invisible'
        }
        if (this.assessment === buttonKey) {
          if (this.assessment === '1') {
            return 'btn-active'
          } else {
            return 'btn-active-wrong'
          }
        } else {
          return 'btn-inactive'
        }
      }
    },
    getSyllableClass() {
      return (syllableNumber) => {
        // if assessment is undefined means this student doesn't need to second check this question
        if (this.assessment === undefined) {
          return null
        } else if (`${this.questionOrder}_${syllableNumber}` in this.assessment) {
          return 'btn-active'
        } else {
          return 'btn-inactive'
        }
      }
    }
  },
  methods: {
    buttonText(key) {
      const text = {
        '1': '1 正確',
        '2': '2 錯誤',
        '3': '3 袂曉',
        '4': '4 不出聲',
        'X': 'X 音檔無聲'
      }

      return text[key]
    },
    syllableText(key) {
      return `音節${parseInt(key)}`
    },
    handleClick(key) {
      this.$emit('assessment-value-update', key)
    },
    startSyllableCorrection(whichSyllable) {
      this.$emit('start-syllable-correction', {
        syllable: whichSyllable,
        oldValue: this.assessment[`${this.questionOrder}_${whichSyllable}`]
      })
    }
  }
}
</script>

<template>
  <tbody>
  <tr>
    <th scope="row" class="th-block">{{ question }}</th>
    <!--        <th scope="row" class="th-block">6年5班 20號</th>-->
    <th scope="row" class="th-block">
      <audio controls style="display: block; margin: 0 auto;">
        <source src='data:audio/wav;base64,${audioString}' type="audio/wav">
      </audio>
    </th>
    <!--  if buttonKeys is Array, it's 1st correction  -->
    <th class="th-block" v-if="Array.isArray(buttonKeys)">
      <button
        v-for="_key in buttonKeys"
        :key="_key"
        :class="getButtonClass(_key)"
        class="btn"
        style="color: white; justify-content: space-evenly"
        @click="handleClick(_key)"
      >{{ buttonText(_key) }}
      </button>
    </th>
    <!--  otherwise, it'll receive a Number as syllable count  -->
    <th class="th-block" v-else>
      <button
        v-for="index in buttonKeys"
        :key="index"
        class="btn"
        :class="getSyllableClass(index)"
        style="color: white; justify-content: space-evenly"
        @click="startSyllableCorrection(index)"
      >{{ syllableText(index) }}
      </button>
    </th>
  </tr>

  </tbody>
</template>

<style scoped>
.btn-active {
  background-color: green;
}

.btn-active-wrong {
  background-color: red;
}

.btn-inactive {
  background-color: rgba(99, 97, 97, 0.35);
}

.btn-invisible {
  visibility: hidden;
}

</style>