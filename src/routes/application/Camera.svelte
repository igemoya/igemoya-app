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

  let barOpened = false;

  let objects = [];

  const loadingShow = () => {
    document.getElementById("loadingContainer").classList.remove("hide");
  };

  const loadingHide = () => {
    document.getElementById("loadingContainer").classList.add("hide");
  };

  const plusClicked = (n) => {
    if(document.getElementsByClassName("selected").length) document.getElementsByClassName("selected")[0].classList.remove("selected");
    document.getElementsByClassName("plusIcon")[n].classList.add("selected");
    document.getElementById("subTitleText").textContent = objects[n].name;
    document.getElementById("explainText").textContent = objects[n].description;
  };

  const barClicked = () => {
    if(barOpened) {
      document.getElementById("bottomExplainContainer").style.marginBottom = "-80vh";
      document.getElementById("floatingInfoContainer").classList.add("show");
    } else {
      document.getElementById("bottomExplainContainer").style.marginBottom = "0vh";
      document.getElementById("floatingInfoContainer").classList.remove("show");
    }
    barOpened = !barOpened;
  };

  const takePhoto = () => {
    loadingShow();
    document.getElementById("bottomToolsContainer").classList.remove("show");
    let video = document.getElementById("webcam");
    let width = video.videoWidth;
    let height = video.videoHeight;
    let canvas = document.createElement("canvas");
    let context = canvas.getContext('2d');
    canvas.width = width;
    canvas.height = height;
    context.drawImage(video, 0, 0, width, height);
    document.getElementById("image").src = canvas.toDataURL();

    navigator.geolocation.getCurrentPosition((position) => {
      const latitude = position.coords.latitude;
      const longitude = position.coords.longitude;

      axios('https://igemoya-backend.herokuapp.com/image-search/image-search', {
        method: 'post',
        headers: {
          authorization: `Bearer ${localStorage.jwt}`
        },
        data: {
          location: {
            type: "Point",
            coordinates: [
              37.57755999577882, 126.97674003790465
            ]
          },
        }
      }).then((res) => {
        document.getElementById("titleText").textContent = res.data.result.name;
        document.getElementById("explainText").textContent = res.data.result.description;
        const id = res.data.result._id;
        const api = "https://pdg916.pythonanywhere.com";
        canvas.toBlob((blob) => {
          const formData = new FormData();
          formData.append('image', blob, 'image.png');
          formData.append('place', idToName[id]);

          // Post via axios or other transport method
          axios.post(`${api}/igemoya/v1/object-detection`, formData)
          .then((res) => {
            let innerElement = "";
            let unique = new Set();
            objects = [];
            res.data.forEach(element => {
              if(!unique.has(element.name)) {
                axios.get(`https://igemoya-backend.herokuapp.com/exhibition-info/object/${element.name}`, {
                  headers: {
                    authorization: `Bearer ${localStorage.jwt}`
                  }
                }).then((res) => {
                  objects.push(res.data.object);
                }).catch(() => {
                  navigate('/oauth/logout', { replace: true });
                });
                unique.add(element.name);
                const x = Math.round((element.xmin + element.xmax) / 2);
                const y = Math.round((element.ymin + element.ymax) / 2);
                const rate = window.innerHeight / height * 0.92;
                const correction = window.innerWidth / 20 > window.innerHeight / 40 ? window.innerHeight / 40 : window.innerWidth / 20;
                const left = rate * x - correction;
                const top = rate * y - correction;
                innerElement += `<div class="plusIcon" style="top: ${top}px; left: ${left}px;">+</div>`;
              }
            });
            document.getElementById("plusContainer").innerHTML = innerElement;
            let buttons = document.getElementsByClassName("plusIcon");
            for(let i = 0; i < buttons.length; i++) {
              buttons[i].addEventListener('click', () => {
                plusClicked(i);
              });
            }
            document.getElementById("bottomExplainContainer").classList.remove("hide");
            loadingHide();
          });
        });
      }).catch(() => {
        alert("Error occured.");
      });
    }, () => {
      alert('Unable to retrieve location');
    });
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
  <div id="floatingInfoContainer" class="show">
    <div id="floatingInfo"><span class="w700">사진을 찍고 궁금한 부분을 터치</span>하세요.</div>
  </div>
  <!-- svelte-ignore a11y-media-has-caption -->
  <div id="videoContainer" class="videoContainer">
    <video id="webcam" height={height} playsinline autoplay></video>
  </div>
  <div id="imgContainer" class="videoContainer">
    <img src="" alt="" id="image">
  </div>
  <div id="plusContainer" class="videoContainer">
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
  <div id="bottomExplainContainer" class="hide">
    <div id="bottomBar" on:click={barClicked}></div>
    <div id="bottomTitle">
      <span id="titleText" class="w800"></span>
      &nbsp;
      <span id="subTitleText" class="w400"></span>
    </div>
    <div id="bottomExplain">
      <span id="explainText"></span>
    </div>
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

  :global(.plusIcon) {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    max-width: 5vh;
    max-height: 5vh;
    width: 10vw;
    height: 10vw;
    background-color: #007CFBdd;
    color: white;
    font-size: 3vh;
    font-weight: 500;
    border-radius: 5vw;
  }

  :global(.selected) {
    background-color: #ffffffdd;
    color: #007CFB;
  }

  #floatingInfoContainer.show {
    opacity: 1;
  }

  #floatingInfoContainer {
    opacity: 0;
    pointer-events: none;
    z-index: 3;
    display: flex;
    width: 100vw;
    justify-content: center;
    align-items: center;
    left: 0;
    top: env(safe-area-inset-top);
    position: fixed;
    transition-duration: .5s;
  }

  .videoContainer {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    height: calc(92vh - env(safe-area-inset-bottom));
  }

  #imgContainer {
    width: 100vw;
    position: fixed;
    top: 0;
  }

  #image {
    height: calc(92vh - env(safe-area-inset-bottom));
  }

  #bottomToolsContainer.show {
    margin-bottom: 0vh;
  }

  #bottomExplainContainer.hide {
    margin-bottom: -92vh;
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

  #bottomExplainContainer {
    margin-bottom: -80vh;
    transition-duration: .5s;
    background-color: #fff;
    position: absolute;
    bottom: calc(8vh + env(safe-area-inset-bottom));
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-direction: column;
    width: 90vw;
    height: 92vh;
    border-radius: 2em 2em 0 0;
    padding-left: 5vw;
    padding-right: 5vw;
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
    height: calc(92vh - env(safe-area-inset-bottom));
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

  #bottomBar {
    background-color: #DBDBDB;
    border-radius: 1vh;
    width: 20vw;
    height: 6px;
    margin-top: 2vh;
  }

  #bottomTitle {
    display: flex;
    width: 100%;
    height: calc(10vh - 6px);
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    font-size: 3.5vh;
  }

  #bottomExplain {
    display: flex;
    width: 100%;
    align-items: flex-start;
    justify-content: flex-start;
    font-size: 2.3vh;
  }
</style>