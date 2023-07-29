<script>
	import { onMount } from "svelte";
	import Loader from "../../components/Loader.svelte";
  onMount(async() => {
    function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
  await sleep(3000);
  localStorage.clear();
  window.location.href = "/login";
  })
  
</script>
<Loader />