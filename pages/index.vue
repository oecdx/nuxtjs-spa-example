<template>
  <v-container fluid>
    <v-row class="pt-12">
      <v-col col6>
        <div class="file-upload">
          <div class="image-upload-wrap" v-if="imageSrc === null">
            <input
              @change="readURL($event)"
              type="file"
              name="file"
              class="file-upload-input"
              accept="image/*"
              ref="uploadInput"
            />
            <div class="drag-text">
              <h3>Drag and drop a file or select add Image</h3>
            </div>
          </div>
          <div class="file-upload-content" v-if="imageSrc != null">
            <img class="file-upload-image" :src="imageSrc" alt="your image" />
            <img
              class="file-upload-image"
              v-if="responseImgSrc != null"
              :src="responseImgSrc"
              alt="response image"
            />
          </div>
          <v-btn class="mt-3" block @click="uploadClick($event)" v-if="imageSrc === null">写真を登録</v-btn>
          <v-btn class="mt-3" block @click="removeUpload" v-if="imageSrc != null">削除</v-btn>
          <v-card class="mt-3" v-for="(result,index) in resultJson" v-bind:key="index">
            <v-card-text v-for="(emotion, subIndex) in result.Emotions" v-bind:key="subIndex">
              <div>感情：{{ emotion.Type | japanize }}</div>
              <div>確信度：{{ emotion.Confidence }}</div>
            </v-card-text>
          </v-card>
          <v-card class="mt-3" v-if="resultJson">
            <v-card-text>
              <list-inner v-if="resultJson" :results="resultJson" :label="''" />
            </v-card-text>
          </v-card>
          <v-card class="mt-3" v-if="resultJson">
            <v-card-title>元データ</v-card-title>
            <v-card-text style="white-space:pre-wrap; word-wrap:break-word;">
              {{
              resultJsonStr
              }}
            </v-card-text>
          </v-card>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ListInner from '~/components/ListInner'

export default {
  data() {
    return {
      title: 'rekognition-test',
      imageSrc: null,
      responseImgSrc: null,
      resultJson: ''
    }
  },
  components: {
    ListInner
  },
  filters: {
    japanize: function(value) {
      var emotionMapping = {
        FEAR: '恐怖',
        HAPPY: '幸福',
        ANGRY: '怒り',
        SAD: '悲しみ',
        DISGUSTED: '嫌悪',
        CALM: '穏やか',
        CONFUSED: '混乱',
        SURPRISED: '驚き'
      }

      if (emotionMapping[value]) {
        return value + '（' + emotionMapping[value] + '）'
      } else {
        return value ? value : ''
      }
    }
  },
  computed: {
    resultJsonStr() {
      if (this.resultJson) {
        return JSON.stringify(this.resultJson, null, 4)
      } else {
        return this.resultJson
      }
    }
  },
  head() {
    return {
      bodyAttrs: {},
      script: [],
      link: []
    }
  },
  mounted() {
    // 子コンポーネントのロードを無視して実行
    this.$nextTick(() => {})
  },
  destroyed: function() {},
  methods: {
    uploadClick($event) {
      const elem = this.$refs.uploadInput
      elem.click()
    },
    readURL(e) {
      let input = e.target
      if (input.files && input.files[0]) {
        let fr = new FileReader()
        fr.onloadend = event => {
          let base64Content = fr.result
          this.imageSrc = base64Content
          // TODO あとでKeyは外部化
          let requestBody = {
            image: base64Content.replace('data:image/jpeg;base64,', '')
          }

          let config = {
            headers: {
              'content-type': 'application/json'
            }
          }
          this.$axios
            // .post('/api/nuxtPost', requestBody, config)
            .post(process.env.AWS_API_URL, requestBody, config)
            .then(response => {
              console.log(response.data.results)
              if (
                response.data.results.FaceDetails &&
                response.data.results.FaceDetails.length > 0
              ) {
                this.resultJson = response.data.results.FaceDetails
              }
            })
            .catch(error => {
              // error 処理
              console.log(JSON.stringify(error))
            })
        }
        fr.readAsDataURL(input.files[0])
      }
    },
    removeUpload() {
      this.imageSrc = null
      this.responseImgSrc = null
      this.resultJson = ''
    }
  }
}
</script>
