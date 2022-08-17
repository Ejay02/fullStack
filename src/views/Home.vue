<script setup>
import { useRouter } from "vue-router";
import Typewriter from "../Components/Typewriter.vue";

const router = useRouter();

const logout = async () => {
  const res = await fetch("http://localhost:3333/logout", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      // Authorization: `Bearer ${localStorage.getItem("token")}`,
    },
    body: JSON.stringify({
      session_token: localStorage.getItem("token"),
    }),
  }).then((res) => res.json());
  if (res.success) {
    localStorage.removeItem("token");
    router.push("/login");
  } else {
    alert(res.message);
  }
};
</script>
<template>
  <main>
    <!-- <h1>home here</h1> -->
    <Typewriter />
    <button @click="logout">Logout</button>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: var(--primary);
  color: #fff;
}

button {
  background-color: #fff;
  color: var(--dark);
  border: 1px solid var(--dark);
  padding: 0.5rem 1rem;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: var(--dark);
  color: #fff;
}

h1 {
  font-size: 2.125rem;
  margin-bottom: 1rem;
}
</style>
