<script>
  import { onMount } from "svelte";
  import Loader from "../../../../components/Loader.svelte";
	import axios from "axios";
	import { page } from "$app/stores";

  let loading = true;
  let name = "";
  let surname = "";
  let birth_date = "";
  let gender = "Male";
  let id_card = "";
  let phone_number = "";
  onMount(async() => {
    try {
      const res = await axios.post("http://139.180.154.56:3021/api/users/" + $page.params.slug);
      name = res.data.name;
      surname = res.data.surname;
      birth_date = res.data.birth_date;
      gender = res.data.gender;
      id_card = res.data.id_card;
      phone_number = res.data.phone_number;
    } catch (error) {
      console.log(error);
    }
    loading = false;
  });

  const handleSubmit = (async(e) => {
    e.preventDefault();
    loading = true;
    await axios.put("http://139.180.154.56:3021/api/users/" + $page.params.slug , {
      name: name,
      surname: surname,
      birth_date: birth_date,
      gender: gender,
      id_card: id_card,
      phone_number: phone_number,
    });
    alert("Yangilandi");
    window.location.href ="/home/users";
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
      <label for="id" class="block mb-2 text-sm font-medium text-gray-90">Pasport Seriya va Raqami</label>
      <input bind:value={id_card} type="text" id="id" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 outline-blue-500" placeholder="AB1231212" required>
    </div>
    <div class="mb-4">
      <label for="phone" class="block mb-2 text-sm font-medium text-gray-90">Telefon Raqami (+ belgisiz)</label>
      <input bind:value={phone_number} type="text" id="phone" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 outline-blue-500" placeholder="998901234567" required>
    </div>
    <input type="submit" value="Submit" class="text-white bg-blue-600 hover:bg-blue-700 font-medium rounded-lg text-sm min-w-full sm:w-auto px-5 py-2.5 text-center" />
  </form>
</div>
{/if}
