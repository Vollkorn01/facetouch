<template>
  <div class="container">
    <div>
      <h1 class="title">
        VIDEO
      </h1>

      <canvas
        id="canvas"
        class="border"
      />
    </div>

    <div class="p20">
      Handtrack.js allows you prototype handtracking interactions in the browser
      in 10 lines of code.
    </div>
    <div class="mb10">
      <button
        id="trackbutton"
        class="bx--btn bx--btn--secondary"
        type="button"
        @click="toggleVideo()"
      >
        Toggle Video
      </button>
      <div
        id="updatenote"
        class="updatenote mt10"
      >
        loading model ..
      </div>
    </div>
    <video
      id="myvideo"
      class="videobox canvasbox"
      autoplay="autoplay"
    />

    <canvas
      id="canvas"
      class="border canvasbox"
    />
  </div>
</template>

<script>
import * as handTrack from 'handtrackjs'
import * as handpose from '@tensorflow-models/handpose'

export default {
  name: 'TensorFlowHandTrack',
  components: {},
  data: function() {
    return {
      model: null
    }
  },
  mounted: function() {
  },

  methods: {



    startVideo(video) {
      // eslint-disable-next-line no-undef
      handTrack.startVideo(video).then((status) => {
        console.log('video started', status)
        if (status) {
          //updateNote.innerText = 'Video started. Now tracking'
          this.runPredictions(video);
          
        } else {
          //updateNote.innerText = 'Please enable video'
        }
      })
    },

    async runPredictions(video) {
        const model = await handpose.load();
          const predictions = await model.estimateHands(video);
          console.log('Predictions: ', predictions)
    },

    toggleVideo() {

      const video = document.getElementById('myvideo')
      const canvas = document.getElementById('canvas')
      const context = canvas.getContext('2d')
      //let updateNote = document.getElementById('updatenote')

      let isVideo = false

      if (!isVideo) {
        //updateNote.innerText = 'Starting video'
        this.startVideo(video, isVideo, canvas, context)
      } else {
        //updateNote.innerText = 'Stopping video'
        this.handTrack.stopVideo(video)
        isVideo = false
        //updateNote.innerText = 'Video stopped'
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
body {
  padding: 20px;
}

.p20 {
  padding: 20px;
}

.canvasbox {
  border-radius: 3px;
  margin-right: 10px;
  width: 450px;
  height: 338px;
  border-bottom: 3px solid #0063ff;
  box-shadow: 0 2px 3px 0 rgba(0, 0, 0, 0.2), 0 4px 10px 0 #00000030;
  background: #333;
}

.mb10 {
  margin-bottom: 10px;
}

.mt10 {
  margin-top: 10px;
}

.updatenote {
  padding: 10px;
  background: rgb(245, 147, 20);
  color: white;
  display: inline;
}
</style>
