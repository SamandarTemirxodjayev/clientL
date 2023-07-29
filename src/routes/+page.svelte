<script>
import { onMount } from 'svelte';
import Loader from '../components/Loader.svelte';
let loading = true;
onMount(async () => {
  const token = localStorage.getItem('token');
  if (!token) {
    window.location.href = '/login';
  }else{
    window.location.href = "/home/users";
  }
  loading = false;
});
</script>
{#if loading}
  <Loader />
{/if}