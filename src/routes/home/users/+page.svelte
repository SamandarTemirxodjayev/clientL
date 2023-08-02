<script>
  import axios from "axios";
	import { onMount } from "svelte";
	import Loader from "../../../components/Loader.svelte";

  let users = {};
  let loading = true;
  onMount(async () => {
    const res = await axios.get("http://139.180.154.56/api/users");
    users = res.data;
    loading = false;
  });
</script>

{#if loading}
  <Loader />
{:else}
<div class="p-4 sm:ml-64">
  <a href="/home/users/new" class="flex justify-end mb-4 mr-10">
    <button class="text-white bg-blue-500 hover:bg-blue-600 py-2 px-3 rounded-sm font-semibold">
      Foydalanuvchi Qo'shish
    </button>
  </a>
  <table class="w-full">
    <thead>
      <tr>
        <th class="px-5 py-3 border border-black">F.I.SH</th>
        <th class="px-5 py-3 border border-black">ID</th>
        <th class="px-5 py-3 border border-black">Telefon Raqami</th>
        <th class="px-5 py-3 border border-black">Detallar</th>
      </tr>
    </thead>
    <tbody>
      {#each users.reverse() as user}
      <tr class="hover:bg-gray-200 cursor-pointer text-center">
        <td class="px-5 py-3 border border-black">{user.name} {user.surname}</td>
        <td class="px-5 py-3 border border-black">{user.id_card}</td>
        <td class="px-5 py-3 border border-black">+{user.phone_number}</td>
        <td class="py-1 border border-black">
          <a href="{`/home/users/${user._id}`}">
            <button class="p-1 rounded-full from-rose-400 via-fuchsia-500 to-indigo-500 bg-gradient-to-r">
              <span class="block text-black px-4 py-2 font-semibold rounded-full bg-white hover:bg-transparent hover:text-white transition">Tahrirlash</span>
            </button>
          </a>
          <a href="{`/home/answers/${user._id}`}">
            <button class="p-1 rounded-full from-rose-400 via-fuchsia-500 to-indigo-500 bg-gradient-to-r">
              <span class="block text-black px-4 py-2 font-semibold rounded-full bg-white hover:bg-transparent hover:text-white transition">Analiz Javoblari</span>
            </button>
          </a>
        </td>
      </tr>
      {/each}
    </tbody>
  </table>
</div>
{/if}