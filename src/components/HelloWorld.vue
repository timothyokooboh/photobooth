<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Photo booth</title>
  <style>
    
    .app-container {
      max-width: 1000px;
      margin: auto;
      text-align: center;
      margin-top: 50px;
    }
    .buttons-container {
      margin-top: 50px;
    }
    button {
      width: 150px;
      padding: 5px;
      font-size: 1.2rem;
      border-radius: 10px;
      color: #fff;
      text-align: center;
      cursor: pointer;
    }
    #start-camera {
      background-color: black;
    }
    #capture-photo {
      background-color: dodgerblue;
    }
  </style>
</head>
<body>
 
  <div class="app-container">
    <video controls autoplay></video>
    <img src="" alt="">
    <canvas style="display:none"></canvas>

    <div class="buttons-container">
      <button onclick="startCamera()" id="start-camera">Start camera</button>
      <button onclick="capturePhoto()" id="capture-photo" >Capture image</button>
      <button id="css-filters-apply" style="color:black">Add css filters</button>
    </div>

    <div id="image-frames-container">

    </div>
  </div>

  <script>
    const video = document.querySelector('video');
    const image = document.querySelector('img');
    const canvas = document.querySelector('canvas');
    const imageFramesContainer = document.querySelector('#image-frames-container');
    const cssFilterButton = document.querySelector('#css-filters-apply');
    let cssFilter = ''

    const constraints = {
      video: true
    }

    let filterIndex = 0;
    let filters = [
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
    ]

    if(navigator.mediaDevices) {
    function startCamera(){
      
        navigator.mediaDevices.getUserMedia(constraints)
      .then((stream)=> {
        video.srcObject = stream
      })
      
    }
    } 
    
    else {
      alert('GetUserMedia not supported')
    }
    

   cssFilterButton.onclick=function(){
     video.className=filters[filterIndex++ % filters.length]
     cssFilter = video.className 
     video.style.filter= cssFilter;
    console.log(cssFilter)
   }

    const emptyArr = [];

    function capturePhoto(){
        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        canvas.getContext('2d').drawImage(video, 0, 0);

        image.src = canvas.toDataURL('image/webp');
        
        image.style.filter= cssFilter;

        const arr = [...emptyArr, image.src] 

        const myImage = document.createElement('img') 
        myImage.style.filter= cssFilter;
        arr.map((value)=>{
          myImage.src=value;
          imageFramesContainer.appendChild(myImage)
        })
    }
  </script>
</body>
</html>


 <!-- <div class="buttons-container">
      <div>
        <button @click="startCamera" id="start">Start Camera</button>
        <select>
          <option value="" selected>Add filters</option>
          <option v-for="(filter, index) in filters" :key="index">{{filter}}</option>
        </select>
      </div>
      <div>
        <button @click="screenshot" id="screenshot">Take screenshot</button>
      </div>     
    </div> -->