<script>
export default {
  emits: ['assessment-value-update'],
  props: {
    question: {
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
    return {
      // buttonKeys: ['1', '2', '3', '4', 'X']
    }
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
    <th class="th-block" v-else>
      <button
        v-for="index in buttonKeys"
        :key="index"
        class="btn btn-primary"
      >{{ syllableText(index) }}</button>
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

.btn-primary {
  color: white;
  background-color: #0d6efd;
}
</style>