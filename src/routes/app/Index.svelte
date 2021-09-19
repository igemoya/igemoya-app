<script lang="ts">
  import { navigate } from "svelte-routing";
  import axios from "axios";

  let username = "";

  axios.get('https://igemoya-backend.herokuapp.com/user/me', {
    headers: {
      authorization: `Bearer ${localStorage.jwt}`
    }
  }).then((res) => {
    username = res.data.username;
  }).catch(() => {
    navigate('/oauth/logout', { replace: true });
  });
</script>