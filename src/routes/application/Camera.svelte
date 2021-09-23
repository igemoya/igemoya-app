<script lang="ts">
  import Navbar from "../../components/Navbar.svelte";
  import { navigate } from "svelte-routing";
  import axios from "axios";

  const height = window.innerHeight;

  let me = {
    username: "",
    profile_image: ""
  };

  let idToName = {
    "6149d45055f0c6001de57d49": "maids",
    "6149600255f0c6001de57cf9": "geunjeongjeon"
  };

  const loadingShow = () => {
    document.getElementById("loadingContainer").classList.remove("hide");
  };

  const loadingHide = () => {
    document.getElementById("loadingContainer").classList.add("hide");
  };

  const takePhoto = () => {
    // loadingShow();
    // document.getElementById("bottomToolsContainer").classList.remove("show");
    let video = document.getElementById("webcam");
    let width = video.videoWidth;
    let height = video.videoHeight;
    let canvas = document.createElement("canvas");
    let context = canvas.getContext('2d');
    canvas.width = width;
    canvas.height = height;
    context.drawImage(video, 0, 0, width, height);
    document.getElementById("image").src = canvas.toDataURL();

    // navigator.geolocation.getCurrentPosition((position) => {
    //   const latitude = position.coords.latitude;
    //   const longitude = position.coords.longitude;

    //   axios('https://igemoya-backend.herokuapp.com/image-search/image-search', {
    //     method: 'post',
    //     headers: {
    //       authorization: `Bearer ${localStorage.jwt}`
    //     },
    //     data: {
    //       location: {
    //         type: "Point",
    //         coordinates: [
    //           37.582442541038596, 126.97410541810576
    //         ]
    //       },
    //     }
    //   }).then((res) => {
    //     const id = res.data.result._id;
    //     const api = "https://pdg916.pythonanywhere.com";
    //     canvas.toBlob((blob) => {
    //       const formData = new FormData();
    //       formData.append('image', blob, 'image.png');
    //       formData.append('place', idToName[id]);

    //       // Post via axios or other transport method
    //       axios.post(`${api}/igemoya/v1/object-detection`, formData)
    //       .then((res) => {
    //         const r = window.innerWidth / 10;
    //         res.data.forEach(element => {
    //           const x = Math.round((element.xmin + element.xmax) / 2);
    //           const y = Math.round((element.ymin + element.ymax) / 2);
    //           console.log(x, y);
    //         });
    //         loadingHide();
    //       });
    //     });
    //   }).catch(() => {
    //     alert("Error occured.");
    //   });
    // }, () => {
    //   alert('Unable to retrieve location');
    // });
  };

  axios.get('https://igemoya-backend.herokuapp.com/user/me', {
    headers: {
      authorization: `Bearer ${localStorage.jwt}`
    }
  }).then((res) => {
    me = res.data;
  }).catch(() => {
    navigate('/oauth/logout', { replace: true });
  });

  navigator.mediaDevices.getUserMedia({
    video: {
      width: { min: 1280, ideal: 1920 },
      height: { min: 720, ideal: 1080 },
      facingMode: { ideal: "environment" }
    }
  }).then(stream => {
    document.getElementById("webcam").srcObject = stream;
  }).catch((error) => {
    alert(error);
  });
</script>

<main>
  <div id="loadingContainer" class="hide">로딩중..</div>
  <div id="floatingInfoContainer">
    <div id="floatingInfo"><span class="w700">사진을 찍고 궁금한 부분을 터치</span>하세요.</div>
  </div>
  <!-- svelte-ignore a11y-media-has-caption -->
  <div id="videoContainer">
    <video id="webcam" height={height} playsinline autoplay></video>
  </div>
  <div id="imgContainer">
    <img src="" alt="" id="image">
  </div>
  <div id="bottomToolsContainer" class="show">
    <div class="bar30"></div>
    <div class="bar40">
      <div id="cameraButton" on:click={takePhoto}>
        <div id="cameraInner">
          <div id="cameraMoreInner"></div>
        </div>
      </div>
    </div>
    <div class="bar30"></div>
  </div>
  <Navbar selected=1 />
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
  }

  #floatingInfo {
    margin-top: 3vh;
    color: #000;
    font-size: 1.5vh;
    filter: drop-shadow(0 0 15px rgba(20, 20, 20, 0.2));
    background-color: #fff;
    padding: 2vw 2vh;
    border-radius: 2vh;
  }

  #floatingInfoContainer {
    display: flex;
    width: 100vw;
    justify-content: center;
    align-items: center;
    left: 0;
    top: env(safe-area-inset-top);
    position: fixed;
  }

  #videoContainer {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    height: calc(92vh - env(safe-area-inset-bottom));
  }

  #imgContainer {
    width: 100vw;
    position: fixed;
    top: 0;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    height: calc(92vh - env(safe-area-inset-bottom));
  }

  #image {
    height: calc(92vh - env(safe-area-inset-bottom));
  }

  #bottomToolsContainer.show {
    margin-bottom: 0vh;
  }

  #bottomToolsContainer {
    margin-bottom: -12vh;
    transition-duration: .5s;
    -webkit-backdrop-filter: blur(10px);
    backdrop-filter: blur(10px);
    background-color: rgba(255, 255, 255, 0.3);
    position: absolute;
    bottom: calc(8vh + env(safe-area-inset-bottom));
    display: flex;
    flex-direction: row;
    height: 12vh;
  }

  video {
    height: calc(92vh - env(safe-area-inset-bottom));
  }
  
  .bar30 {
    width: 30vw;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .bar40 {
    width: 40vw;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  #cameraButton {
    filter: drop-shadow(0 0 15px rgba(20, 20, 20, 0.4));
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #fff;
    width: 7vh;
    height: 7vh;
    border-radius: 7vh;
  }

  #cameraInner {
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(to bottom right, #5dc5f5, #f55df0);
    width: 6vh;
    height: 6vh;
    border-radius: 6vh;
  }

  #cameraMoreInner {
    background-color: #fff;
    width: 5vh;
    height: 5vh;
    border-radius: 5vh;
  }

  #loadingContainer {
    z-index: 5;
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    padding-top: env(safe-area-inset-top);
    padding-bottom: env(safe-area-inset-bottom);
    background-color: rgba(20, 20, 20, 0.5);
    font-size: 3vh;
    font-weight: 600;
    color: white;
    opacity: 1;
    transition-duration: .5s;
  }

  #loadingContainer.hide {
    pointer-events: none;
    opacity: 0;
  }
</style>