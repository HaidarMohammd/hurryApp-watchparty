<template>
  <h2
    class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900"
  >
    Sign in to your account
  </h2>

  <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
    <div class="space-y-6">
      <div>
        <label
          for="email"
          class="block text-sm font-medium leading-6 text-gray-900"
          >Email address</label
        >
        <div class="mt-2">
          <input
            v-model="email"
            id="email"
            name="email"
            type="email"
            autocomplete="email"
            required
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div>
        <div class="flex items-center justify-between">
          <label
            for="password"
            class="block text-sm font-medium leading-6 text-gray-900"
            >Password</label
          >
        </div>
        <div class="mt-2">
          <input
            v-model="password"
            id="password"
            name="password"
            type="password"
            autocomplete="current-password"
            required
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div>
        <button
          @click="login"
          class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
        >
          Login
        </button>
      </div>
    </div>

    <p class="mt-10 text-center text-sm text-gray-500">
      Not a member?
      <router-link
        :to="{ name: 'Register' }"
        class="font-semibold leading-6 text-indigo-600 hover:text-indigo-500"
        >Register</router-link
      >
    </p>
  </div>
</template>

<script setup>
import axios from "../axios";
import { useUserStore } from "../stores/userStore";
import { computed, ref } from "vue";
import { useRouter } from "vue-router";
import { toast } from "vue-sonner";

const email = ref();
const password = ref();
const router = useRouter();
const user = useUserStore();

const login = async () => {
  try {
    const { data } = await axios.post(
      "/users/login",
      {
        email: email.value,
        password: password.value,
      },
      {
        withCredentials: true,
      }
    );

    if (data.user) {
      user.user = data.user;
      localStorage.setItem("token", data.token);
      router.push({ name: "Main" });
    }
  } catch (e) {
    if (e.response?.data?.error) {
      return toast.error(e.response.data.error);
    }
    return toast.error("Something went wrong");
  }
};
</script>
