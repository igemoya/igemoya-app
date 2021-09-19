<script>
  import { Router, Route } from "svelte-routing";
  import Main from "./routes/Index.svelte";
  import OAuth from "./routes/oauth/Index.svelte";
  import App from "./routes/app/Index.svelte";

  export let url = "";
</script>

<Router url="{url}">
  <div>
    <!-- / -->
    <Route path="/"><Main /></Route>
    <!-- /oauth -->
    <Route path="/oauth"><OAuth /></Route>
    <!-- /app -->
    <Route path="/app"><App /></Route>
  </div>
</Router>

<style>
@import url("https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/variable/pretendardvariable.css");

:global(body) {
  overflow-x: hidden;
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