<script>
  import { onMount } from "svelte";
  import Loader from "../../components/Loader.svelte";
  
  let username = '';
  let password = '';
  let error = false;
  let errorName = '';
  let loading = true;
  onMount(async() => {
    if(localStorage.getItem("token")){
      window.location.href = "/";
    }
    loading = false;
  });
  
  const handleSubmit = () => {
    if(!username ||!password) {
      errorName = 'Username va/yoki parolni kiriting';
      error = true;
      return;
    }
    if(username != "admin"){
      errorName = 'Username xato';
      error = true;
      return;
    }
    if(password != "admin"){
      errorName = 'Parol xato';
      error = true;
      return;
    }
    localStorage.setItem('token', username);
    window.location.href = '/';
  }
  </script>
  
  {#if loading}
    <Loader />
  {:else}
    <div class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
      <div class="max-w-md w-full space-y-8">
        <div>
          <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
            Login
          </h2>
        </div>
        <form class="mt-8 space-y-6" on:submit|preventDefault={handleSubmit}>
          {#if error}
            <div class="text-red-900 bg-red-200 py-4 px-2 rounded-md">
              {errorName}
            </div>
          {/if}
          <div class="rounded-md shadow-sm -space-y-px">
            <div>
              <label for="username" class="sr-only">Username</label>
              <input
                id="username"
                name="username"
                type="text"
                class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                placeholder="Username"
                bind:value={username}
                autocomplete="current-password"
              />
            </div>
            <div>
              <label for="password" class="sr-only">Password</label>
              <input
                id="password"
                name="password"
                type="password"
                class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                placeholder="Password"
                bind:value={password}
                autocomplete="current-password"
              />
            </div>
          </div>
  
          <div>
            <button
              type="submit"
              class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              Submit
            </button>
          </div>
        </form>
      </div>
    </div>
  {/if}
  