<template>
  <div class="register-form">
    <h2>Registration Form</h2>
    <form @submit.prevent="register">
      <div>
        <label for="name">Name</label>
        <input id="name" type="text" v-model="user.name" required />
      </div>
      <div>
        <label for="email">Email</label>
        <input id="email" type="email" v-model="user.email" required />
      </div>
      <div>
        <label for="password">Password</label>
        <input id="password" type="password" v-model="user.password" required />
      </div>
      <div>
        <button type="submit">Register</button>
      </div>
    </form>
  </div>
</template>

<script>
import { ref, onMounted, watch, onBeforeUnmount } from "vue";
import "./css/RegisterForm.css";

export default {
  setup() {
    const user = ref({
      name: "",
      email: "",
      password: "",
    });

    const users = ref([]);

    onMounted(() => {
      users.value = JSON.parse(localStorage.getItem("users")) || [];
      const persistedUser = JSON.parse(localStorage.getItem("currentUser"));
      if (persistedUser) {
        user.value = persistedUser;
      }
    });

    watch(user, (newUser) => {
      localStorage.setItem("currentUser", JSON.stringify(newUser));
    });

    const saveUserBeforeUnload = () => {
      localStorage.setItem("currentUser", JSON.stringify(user.value));
    };

    window.addEventListener("beforeunload", saveUserBeforeUnload);

    onBeforeUnmount(() => {
      window.removeEventListener("beforeunload", saveUserBeforeUnload);
    });

    const register = () => {
      users.value.push({ ...user.value });
      localStorage.setItem("users", JSON.stringify(users.value));

      alert("Registration successful!");

      user.value = { name: "", email: "", password: "" };

      localStorage.removeItem("currentUser");
    };

    return {
      user,
      register,
      users,
    };
  },
};
</script>
