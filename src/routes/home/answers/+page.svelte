<script>
  import axios from "axios";
	import { onMount } from "svelte";
	import Loader from "../../../components/Loader.svelte";

  let users = {};
  let loading = true;
  
  onMount(async () => {
    const res = await axios.get("http://139.180.154.56:3021/api/answers");
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
        <td class="px-5 py-3 border border-black">{user.uuid}</td>
        <td class="px-5 py-3 border border-black">{user.id.name} {user.id.surname}</td>
        <td class="px-5 py-3 border border-black">
          <a href="{`http://139.180.154.56:3021/public/${user.uuid}.pdf`}">
            <button class="p-1 rounded-full from-rose-400 via-fuchsia-500 to-indigo-500 bg-gradient-to-r">
              <span class="block text-black px-4 py-2 font-semibold rounded-full bg-white hover:bg-transparent hover:text-white transition">Yuklab Olish</span>
            </button>
          </a>
        </td>
        <td class="px-5 py-3 border border-black text-center">{formatDateToDDMMYYHHMM(user.date)}</td>
        <td class="px-5 py-3 border border-black">+{user.id.phone_number}</td>
      </tr>
      {/each}
    </tbody>
  </table>
</div>
{/if}