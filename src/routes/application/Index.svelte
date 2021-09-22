<script lang="ts">
  import Recommend from "../../components/Recommend.svelte";
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
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
  }

  #topContainer {
    width: 100vw;
    height: 20vh;
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
    height: 7vh;
    border-radius: 5vh;
  }
</style>