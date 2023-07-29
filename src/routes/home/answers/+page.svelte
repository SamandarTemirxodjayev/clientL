<script>
  import axios from "axios";
	import { onMount } from "svelte";
	import Loader from "../../../components/Loader.svelte";

  let users = {};
  let loading = true;
  
  onMount(async () => {
    const res = await axios.get("http://139.84.171.101/api/answers");
    users = res.data
    loading = false;
  });
  function formatDateToDDMMYYHHMM(dateString) {
  const date = new Date(dateString);

  const day = String(date.getDate()).padStart(2, "0");
  const month = String(date.getMonth() + 1).padStart(2, "0");
  const year = String(date.getFullYear()).slice(-2);
  const hours = String(date.getHours()).padStart(2, "0");
  const minutes = String(date.getMinutes()).padStart(2, "0");

  return `${hours}:${minutes} ${day}.${month}.${year}`;
}
</script>

{#if loading}
  <Loader />
{:else}
<div class="p-4 sm:ml-64">
  <table class="w-full">
    <thead>
      <tr>
        <th class="px-5 py-3 border border-black">ID</th>
        <th class="px-5 py-3 border border-black">F.I.SH</th>
        <th class="px-5 py-3 border border-black">Analiz Javobi</th>
        <th class="px-5 py-3 border border-black">Sana Oy Yil</th>
        <th class="px-5 py-3 border border-black">Telefon Raqami</th>
      </tr>
    </thead>
    <tbody>
      {#each users.reverse() as user}
      <tr class="hover:bg-gray-200 cursor-pointer text-center">
        <td class="px-5 py-3 border border-black">{user._id}</td>
        <td class="px-5 py-3 border border-black">{user.id.name} {user.id.surname} {user.id.fatherName}</td>
        <td class="px-5 py-3 border border-black">{user.uuid}</td>
        <td class="px-5 py-3 border border-black text-center">{formatDateToDDMMYYHHMM(user.date)}</td>
        <td class="px-5 py-3 border border-black">+{user.id.phone_number}</td>
      </tr>
      {/each}
    </tbody>
  </table>
</div>
{/if}