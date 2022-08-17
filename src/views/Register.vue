<script setup>
import { ref } from "@vue/reactivity";
import { useRouter } from "vue-router";

const email = ref("");
const password = ref("");
const router = useRouter();
const conf_password = ref("");

const register = async () => {
  if (!email.value || !password.value || !conf_password.value) {
    return alert("Please fill all fields");
  }

  if (password.value !== conf_password.value) {
    return alert("Passwords do not match");
  }

  const res = await fetch("http://localhost:3333/register", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      email: email.value,
      password: password.value,
    }),
  }).then((res) => res.json());
  if (res.success) {
    localStorage.setItem("token", res.token);
    router.push("/");
  } else {
    alert(res.message);
  }
};
</script>
<template>
  <main>
    <header>
      <h1 class="logo">SECRET</h1>

      <h2>Register</h2>
      <p>Login or create an account to start using super secret ㊙️</p>
    </header>
    <form @submit.prevent="register">
      <!-- Email field -->
      <label>
        <span>Enter your email</span>
        <input
          type="email"
          v-model="email"
          placeholder="example@example.com"
          required
        />
      </label>

      <!-- Password field -->
      <label>
        <span>Enter your password</span>
        <input
          type="password"
          v-model="password"
          placeholder="********"
          required
        />
        <i class="fas fa-eye-slash"></i>
      </label>

      <!-- Confirm Password field -->
      <label>
        <span>Confirm password</span>
        <input
          type="password"
          v-model="conf_password"
          placeholder="********"
          required
        />
      </label>

      <!-- Submit button -->
      <input type="submit" value="Register" />
    </form>

    <footer>
      <p>
        Already have an account?
        <router-link to="/login">Login</router-link>
      </p>
    </footer>
  </main>
</template>

<style scoped>
.logo {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex;
  background-color: var(--primary);
  color: #fff;
}
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  height: 100vh;
  background-color: var(--primary);
  color: #fff;
}

header {
  padding: 1.5rem;
}

footer {
  background-color: #fff;
  width: 100%;
  color: var(--dark);
  text-align: center;
  padding: 1.5rem;
  padding-bottom: 3rem;
}

h2 {
  font-size: 2.125rem;
  margin-bottom: 1rem;
  text-align: center;
}

h2 ~ p {
  font-weight: 500;
  font-size: 1rem;
}

form {
  flex: 1 1 0%;
  display: block;
  border-radius: 1.5rem 1.5rem 0 0;
  background-color: #fff;
  color: var(--dark);
  padding: 4rem 1.5rem;
  width: 100%;
  box-shadow: 0px -4px 12px 4px rgba(0, 0, 0, 0.16);
}

label {
  display: block;
  margin-bottom: 1.5rem;
}

label span {
  display: block;
  color: var(--gray);
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

input:not([type="submit"]) {
  display: block;
  width: 100%;
  /* border: 1px solid var(--gray); */
  border-radius: 0.5rem;
  padding: 1.5rem 1rem;
  font-size: 1rem;
  font-weight: 500;
  color: var(--dark);
  background-color: var(--light);
  /* transition: border-color 0.2s ease-in-out; */
  margin-bottom: 1.5rem;
}

input:not([type="submit"])::placeholder {
  color: var(--gray);
  font-style: italic;
}

input[type="submit"] {
  display: block;
  width: fit-content;
  margin: 0 auto;
  font-size: 1.5rem;
  font-weight: 700;
  color: #fff;
  background-color: var(--primary);
  padding: 1rem;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: 0.2s ease;
}

input[type="submit"]:hover {
  background-color: var(--primary-dark);
}
</style>
