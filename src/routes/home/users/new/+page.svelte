<script>
  import { onMount } from "svelte";
  import Loader from "../../../../components/Loader.svelte";
	import axios from "axios";

  let loading = true;
  let name = "";
  let surname = "";
  let id_card = "";
  let phone_number = "";
  onMount(() => {
    loading = false;
  });

  const handleSubmit = (async(e) => {
    e.preventDefault();
    loading = true;
    try {
      await axios.put("http://139.84.171.101/api/users", {
      name: name,
      surname: surname,
      id_card: id_card,
      phone_number: "998" + phone_number,
    });
    window.location.href ="/home/users";
    } catch (error) {
      alert(error.message);
    }
    loading = false;

  });
</script>

{#if loading}
  <Loader />
{:else}
<div class="p-4 sm:ml-64 flex items-center justify-center max-w-full">
  <form class="w-[50%]" on:submit={handleSubmit}>
    <div class="mb-4">
      <label for="username" class="block mb-2 text-sm font-medium text-gray-90">Foydalanuvchi Ismi</label>
      <input bind:value={name} type="text" id="username" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 outline-blue-500" required>
    </div>
    <div class="mb-4">
      <label for="lastname" class="block mb-2 text-sm font-medium text-gray-90">Foydalanuvchi Familiyasi</label>
      <input bind:value={surname} type="text" id="lastname" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 outline-blue-500" required>
    </div>
    <div class="mb-4">
      <label for="id" class="block mb-2 text-sm font-medium text-gray-90">ID raqami</label>
      <input bind:value={id_card} type="text" id="id" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 outline-blue-500" placeholder="AB1231212" required>
    </div>
    <div class="mb-4">
      <label for="phone" class="block mb-2 text-sm font-medium text-gray-90">Telefon Raqami</label>
      <div class="flex items-center">
        <div class="bg-gray-200 p-2.5 text-sm border-t border-l border-b border-gray-300">+998</div>
        <input bind:value={phone_number} type="text" id="phone" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-r-lg block w-full p-2.5 outline-blue-500" placeholder="901234567" required>
      </div>
    </div>
    <input type="submit" value="Submit" class="text-white bg-blue-600 hover:bg-blue-700 font-medium rounded-lg text-sm min-w-full sm:w-auto px-5 py-2.5 text-center" />
  </form>
</div>
{/if}
