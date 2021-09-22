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
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
  }
</style>