<script>
  import { Router, Route } from "svelte-routing";
  import Main from "./routes/Index.svelte";
  import OAuth from "./routes/oauth/Index.svelte";
  import Logout from "./routes/oauth/Logout.svelte";
  import App from "./routes/application/Index.svelte";
  import Camera from "./routes/application/Camera.svelte";
  import NotFound from "./routes/NotFound.svelte";

  export let url = "";
</script>

<Router url="{url}">
  <div>
    <!-- / -->
    <Route path="/"><Main /></Route>
    <!-- /oauth -->
    <Route path="/oauth"><OAuth /></Route>
    <Route path="/oauth/logout"><Logout /></Route>
    <!-- /app -->
    <Route path="/app"><App /></Route>
    <Route path="/app/camera"><Camera /></Route>
    <!-- 404 -->
    <Route path=""><NotFound /></Route>
  </div>
</Router>

<style>
@import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/variable/pretendardvariable.css");

div {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  overflow-x: hidden;
  overflow-y: hidden;
}

:global(*) {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  user-select: none;
}

:global(a) {
  line-height: 0;
}

:global(body) {
  font-family: "Pretendard Variable";
  padding: 0;
  margin: 0;
}

:global(.clickable) {
  cursor: pointer;
}

:global(.underline) {
  text-decoration: underline;
}

:global(.w900) {
  font-weight: 900;
}

:global(.w800) {
  font-weight: 800;
}

:global(.w700) {
  font-weight: 700;
}

:global(.w600) {
  font-weight: 600;
}

:global(.w500) {
  font-weight: 500;
}

:global(.w400) {
  font-weight: 400;
}

:global(.w300) {
  font-weight: 300;
}

:global(.w200) {
  font-weight: 200;
}

:global(.w100) {
  font-weight: 100;
}
</style>