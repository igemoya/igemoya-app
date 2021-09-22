<script lang="ts">
  import { navigate } from "svelte-routing";
  import "../@type/index.d.ts";
  
  if(!Kakao.Auth) {
    Kakao.init("f246de8d71e0c8320cb967a7361ae9fe");
  }

  const loginWithKakao = () => {
    Kakao.Auth.authorize({
      redirectUri: 'https://ige-app.coupy.dev/oauth'
    });
  };

  document.body.onload = () => {
    if(localStorage.jwt) navigate("/app", { replace: true });
  };
</script>

<main>
  <img src="/images/igemoya.svg" alt="igemoya" id="logo">
  <span id="logoText" class="w900">이게모야</span>
  <a id="kakaoButton" on:click={loginWithKakao}>
    <img src="/images/kakao_login.png" id="kakao" alt="Kakao Login"/>
  </a>
  <p>본 서비스는 <span class="w700">카카오 로그인</span>을 통해 사용자를 식별합니다.<br>
    로그인 시 <span class="w700">이게모야</span>의 <span class="w700 underline">이용약관</span>과 <span class="w700 underline">개인정보처리방침</span>에 동의하는 것으로 간주됩니다.</p>
</main>

<style>
  main {
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  #logo {
    width: 10vh;
  }

  #logoText {
    margin-top: 1vh;
    font-size: 3vh;
    color: #007cfb;
  }

  #kakaoButton {
    cursor: pointer;
    margin-top: 10vh;
  }

  #kakao {
    width: 20vh;
  }

  p {
    text-align: center;
    color: #dbdbdb;
    font-size: 0.6em;
  }
</style>