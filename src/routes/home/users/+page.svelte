<script>
  import axios from "axios";
	import { onMount } from "svelte";
	import Loader from "../../../components/Loader.svelte";

  let users = {};
  let loading = true;
  onMount(async () => {
    const res = await axios.get("http://139.84.171.101/api/users");
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
        <th class="px-5 py-3 border border-black">ID</th>
        <th class="px-5 py-3 border border-black">F.I.SH</th>
        <th class="px-5 py-3 border border-black">Pasport ID</th>
        <th class="px-5 py-3 border border-black">Tug'ilgan Sana Oy Yil</th>
        <th class="px-5 py-3 border border-black">Jinsi</th>
        <th class="px-5 py-3 border border-black">Telefon Raqami</th>
      </tr>
    </thead>
    <tbody>
      {#each users.reverse() as user}
      <tr class="hover:bg-gray-200 cursor-pointer text-center">
        <td class="px-5 py-3 border border-black">{user._id}</td>
        <td class="px-5 py-3 border border-black">{user.name} {user.surname} {user.fatherName}</td>
        <td class="px-5 py-3 border border-black">{user.id_card}</td>
        <td class="px-5 py-3 border border-black text-center">{user.birth_date}</td>
        <td class="px-5 py-3 border border-black">{user.gender == "Male" ? `Erkak` : `Ayol`}</td>
        <td class="px-5 py-3 border border-black">+{user.phone_number}</td>
      </tr>
      {/each}
    </tbody>
  </table>
</div>
{/if}