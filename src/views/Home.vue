<template>
  <div>
     <div class="buttons-container">
       <div>
         <button @click="startCamera" id="start">Start Camera</button>
         <select @change="selectedFilter" ref="select">
           <option value="" selected disabled>Add filters</option>
           <option v-for="(filter, index) in filters" :key="index" :value="filter">{{filter}}</option>
         </select>
       </div>
       <div>
         <button @click="screenshot" id="screenshot">Take screenshot</button>
       </div>
     </div>
    <div class="media-container">
      <div ref="imageFramesContainer">
        <ul v-for="(item, index) in newArr" :key="index">
          <li>
            <img :src="item.src" class="frame" ref="frame" :style="{filter:item.filter}"> <br>
            <span class="icons" @click="removeFrame(index)">&#9986;
            </span> <span class="icons heart">&#x2764;</span>
          </li>
        </ul>
      </div>
        <video ref="video" controls autoplay></video>
        <img ref="image" src="">
      </div>
      <canvas ref="canvas" style="display:none"></canvas>
  </div>
</template>

<script>
export default {
  data () {
    return {
      constraints: {
        video: { width: 300, height: 200 }
      },
      emptyArr: [],
      newArr: [],
      filters: [
        'grayscale(100%)',
        'sepia(100%)',
        'blur(100%)',
        'brightness(100%)',
        'contrast(100%)',
        'hue-rotate(200deg)',
        'hue-rotate(300deg)',
        'saturate(0%)',
        'invert(100%)',
        'invert(200%)',
        ''
      ],
      chosenFilter: ''

    }
  },
  methods: {
    startCamera () {
      if (navigator.mediaDevices.getUserMedia) {
        navigator.mediaDevices.getUserMedia(this.constraints)
          .then((stream) => {
            this.$refs.video.srcObject = stream
          })
      }
    },
    screenshot () {
      const canvas = this.$refs.canvas
      const video = this.$refs.video
      const image = this.$refs.image
      canvas.width = video.videoWidth
      canvas.height = video.videoHeight
      canvas.getContext('2d').drawImage(video, 0, 0)
      image.src = canvas.toDataURL('image/webp')
      this.newArr.push({
        src: image.src,
        filter: this.chosenFilter
      })
    },
    selectedFilter () {
      const select = this.$refs.select
      const cssFilter = select.selectedOptions[0].value
      this.chosenFilter = cssFilter
      console.log(this.chosenFilter)
      const image = this.$refs.image
      const video = this.$refs.video
      image.style.filter = cssFilter
      video.style.filter = cssFilter
    },
    removeFrame (index) {
      if (confirm('Are you sure you want to delete this image?')) {
        this.newArr.splice(index, 1)
      }
    }
  }
}
</script>
<style scoped lang="scss">
  .buttons-container {
    display: grid;
    justify-content: center;
    grid-template-columns: auto auto;
    column-gap: 20px;
    margin-bottom: 20px;
  }
  button {
    padding: 10px;
    border-radius: 4px;
    cursor: pointer;
    color: #fff;
    text-align: center;
    font-size: 16px;
    outline: none;
  }
  #start {
      background-color: #000;
      margin-right: 10px;
    }
  #screenshot {
      background-color: dodgerblue;
    }
  .media-container {
    display: grid;
    justify-content: center; // You may have to remove this property
    grid-template-columns: 200px 300px 300px;
    column-gap: 50px;
  }
  .frame {
    width: 150px;
  }

  .icons {
    font-size: 20px;
    cursor: pointer;
  }
  .heart {
    color: red;
    margin-left: 100px;
  }
</style>
