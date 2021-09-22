<script lang="ts">
  import Navbar from "../../components/Navbar.svelte";
  import { navigate } from "svelte-routing";
  import axios from "axios";

  const height = window.innerHeight;

  let me = {
    username: "",
    profile_image: ""
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
      width: { ideal: 4096 },
      height: { ideal: 2160 },
      facingMode: { ideal: "environment" }
    }
  }).then(stream => {
    document.getElementById("webcam").srcObject = stream;
  }).catch((error) => {
    console.error(error);
    console.log("에러 발생!");
  });
</script>

<main>
  <!-- svelte-ignore a11y-media-has-caption -->
  <div id="floatingInfoContainer">
    <div id="floatingInfo"><span class="w700">사진을 찍고 궁금한 부분을 터치</span>하세요.</div>
  </div>
  <div id="videoContainer">
    <video id="webcam" height={height} playsinline autoplay></video>
  </div>
  <div id="bottomToolsContainer">
    <div class="bar30"></div>
    <div class="bar40">
      <div id="cameraButton">
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
    color: #000;
    font-size: 1.5vh;
    filter: drop-shadow(0 0 15px rgba(20, 20, 20, 0.2));
    background-color: #fff;
    padding: 2vw 2vh;
    border-radius: 2vh;
  }

  #floatingInfoContainer {
    width: 100vw;
    display: flex;
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

  #bottomToolsContainer {
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
</style>