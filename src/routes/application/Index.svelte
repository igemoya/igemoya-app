<script lang="ts">
  import Recommend from "../../components/Recommend.svelte";
  import Navbar from "../../components/Navbar.svelte";
  import { navigate } from "svelte-routing";
  import axios from "axios";

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

  navigator.geolocation.getCurrentPosition((position) => {
    const latitude = position.coords.latitude;
    const longitude = position.coords.longitude;

    console.log(`Latitude: ${latitude}, Longitude: ${longitude}`);
  }, () => {
    alert('Unable to retrieve location');
  });

  const recommends = {
    "location": {
      "title": "경복궁",
      "recommends": [
        {
          "title": "현판의 비밀",
          "src": "https://www.cha.go.kr/unisearch/imagefiles/newsfile/20100216/VC3K9851.jpg",
          "id": "89ac5fv2br"
        },
        {
          "title": "현판의 비밀",
          "src": "https://www.cha.go.kr/unisearch/imagefiles/newsfile/20100216/VC3K9851.jpg",
          "id": "89ac5fv2br"
        },
        {
          "title": "현판의 비밀",
          "src": "https://www.cha.go.kr/unisearch/imagefiles/newsfile/20100216/VC3K9851.jpg",
          "id": "89ac5fv2br"
        }
      ]
    },
    "around": {
      "title": "경복궁",
      "recommends": [
        {
          "title": "현판의 비밀",
          "src": "https://www.cha.go.kr/unisearch/imagefiles/newsfile/20100216/VC3K9851.jpg",
          "id": "89ac5fv2br"
        },
        {
          "title": "현판의 비밀",
          "src": "https://www.cha.go.kr/unisearch/imagefiles/newsfile/20100216/VC3K9851.jpg",
          "id": "89ac5fv2br"
        },
        {
          "title": "현판의 비밀",
          "src": "https://www.cha.go.kr/unisearch/imagefiles/newsfile/20100216/VC3K9851.jpg",
          "id": "89ac5fv2br"
        }
      ]
    }
  };
</script>

<main>
  <div id="topContainer">
    <div id="topNameContainer">
      {#if me.username}
        <span><span class="w800">{me.username}</span>님 환영합니다!</span>
        <img src="{me.profile_image}" alt="" id="profileImage">
      {/if}
    </div>
  </div>
  <img src="/images/banner.png" alt="" id="bannerImage">
  <div class="contentsContainer">
    <span class="contentsTitle">지금 <span class="w800">경복궁</span>에 있으신가요?</span>
    <div class="contentsInnerContainer">
      {#each recommends.location.recommends as e}
        <Recommend title={e.title} src={e.src} id={e.id} />
      {/each}
      <div id="contentsRight">&nbsp;</div>
    </div>
  </div>
  <div class="contentsContainer">
    <span class="contentsTitle"><span class="w800">{me.username}님 주변</span> 살펴보기</span>
    <div class="contentsInnerContainer">
      {#each recommends.around.recommends as e}
        <Recommend title={e.title} src={e.src} id={e.id} />
      {/each}
      <div id="contentsRight">&nbsp;</div>
    </div>
  </div>
  <Navbar selected=0 />
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
  }

  #bannerImage {
    border: solid 1px #ACACAC;
    border-radius: 10px;
    margin: 4vh 4vw 0vh 4vw;
    width: 92vw;
  }

  #topContainer {
    width: 100vw;
    height: 22vh;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    background-color: #007CFB;
  }

  #topNameContainer {
    color: white;
    font-size: 3vh;
    width: 100vw;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2vh 4vw;
  }

  #profileImage {
    object-fit: cover;
    width: 7vh;
    height: 7vh;
    border-radius: 5vh;
  }

  .contentsContainer {
    width: 100vw;
    padding-top: 4vh;
  }

  .contentsTitle {
    font-size: 2.5vh;
    padding: 0 4vw;
  }

  .contentsInnerContainer {
    padding-left: 4vw;
    overflow-x: scroll;
    margin-top: 2vh;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
  }

  #contentsRight {
    width: 2vw;
  }
</style>