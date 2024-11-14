<template>
  <div class="register-form">
    <h2>Registration Form</h2>
    <form @submit.prevent="register">
      <div>
        <label for="name">Name</label>
        <input type="text" v-model="user.name" required />
      </div>
      <div>
        <label for="email">Email</label>
        <input type="email" v-model="user.email" required />
      </div>
      <div>
        <label for="password">Password</label>
        <input type="password" v-model="user.password" required />
      </div>
      <button type="submit">Register</button>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import "./css/RegisterForm.css";

export default {
  setup() {
    const user = ref({
      name: "",
      email: "",
      password: "",
    });

    const register = () => {
      const users = JSON.parse(localStorage.getItem("users")) || [];
      users.push({ ...user.value });
      localStorage.setItem("users", JSON.stringify(users));

      alert("Registration successful!");
      user.value = { name: "", email: "", password: "" };
    };

    return {
      user,
      register,
    };
  },
};
</script>
