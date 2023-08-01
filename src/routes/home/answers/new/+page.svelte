<script>
  import { onMount } from "svelte";
  import Loader from "../../../../components/Loader.svelte";
  import axios from "axios";

  let loading = true;
  let users = {};
  let id = "";
  let fileToUpload = null;
  let disabled;
  $: disabled = !fileToUpload ? "disabled" : "";

  onMount(async () => {
    try {
      const res = await axios.get("http://139.180.154.56:3021/api/users");
      users = res.data;
      loading = false;
    } catch (error) {
      console.error("Error fetching users:", error);
    }
  });

  const handleFileChange = (event) => {
    if (event.target.files && event.target.files.length > 0) {
      fileToUpload = event.target.files[0];
    }
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    loading = true;
    if (fileToUpload) {
      let formData = new FormData();
      formData.append("size", fileToUpload.size);
      formData.append("file", fileToUpload);
      formData.append("mimeType", fileToUpload.type);
      formData.append("id", id);

      try {
        const res = await axios.put("http://139.180.154.56:3021/api/answers", formData);
        id = "";
        alert("Muvaffiqatli saqlandi");
      } catch (error) {
        console.log(error);
      }
    }
    loading = false;
  };
</script>

{#if loading}
  <Loader />
{:else}
  <div class="p-4 sm:ml-64 flex items-center justify-center max-w-full">
    <form class="w-[50%]" on:submit={handleSubmit}>
      <div class="mb-4">
        <label for="gender" class="block mb-2 text-sm font-medium text-gray-900">Foydalanuvchini tanlang</label>
        <input bind:value={id} type="text" id="browser" list="users" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 outline-blue-500" required />
        <datalist id="users">
          {#each users as user}
            <option value="{user._id}">{user.name} {user.surname}</option>
          {/each}
        </datalist>
      </div>
      <div class="mb-4">
        <div class="flex items-center justify-center w-full">
          <label for="dropzone-file" class="flex flex-col items-center justify-center w-full h-64 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer bg-gray-50  hover:bg-gray-100">
            <div class="flex flex-col items-center justify-center pt-5 pb-6">
              <svg
                class="w-8 h-8 mb-4 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 16"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M13 13h3a3 3 0 0 0 0-6h-.025A5.56 5.56 0 0 0 16 6.5 5.5 5.5 0 0 0 5.207 5.021C5.137 5.017 5.071 5 5 5a4 4 0 0 0 0 8h2.167M10 15V6m0 0L8 8m2-2 2 2"
                ></path>
              </svg>
              {#if fileToUpload}
                <p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
                  File: {fileToUpload.name}
                </p>
              {:else}
                <p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
                  <span class="font-semibold">Click to upload</span> or drag and drop
                </p>
              {/if}
              <p class="text-xs text-gray-500 dark:text-gray-400">Only PDF</p>
            </div>
            <input id="dropzone-file" type="file" class="hidden" required bind:this={fileToUpload} on:change={handleFileChange} />
          </label>
        </div>
      </div>
      <input type="submit" value="Submit" class="text-white bg-blue-600 hover:bg-blue-700 font-medium rounded-lg text-sm min-w-full sm:w-auto px-5 py-2.5 text-center cursor-pointer" />
    </form>
  </div>
{/if}
