<template>
  <Section v-if="showLogin">
    <form @submit.prevent="login()">
      <div class="text-center">
        <ul>
          <li v-for="error in errors" :key="error">{{ error }}</li>
        </ul>
      </div>
      <div class="grid grid-cols-2">
        <div class="flex flex-col gap-2 my-2 items-center">
          <div>
            <input type="email" placeholder="Email" v-model="loginParams.email" class="text-black rounded px-1" />
          </div>
          <div>
            <input
              type="password"
              placeholder="Password"
              v-model="loginParams.password"
              class="text-black rounded px-1"
            />
          </div>
        </div>

        <div class="text-center my-auto">
          <button type="submit" class="border text-black font-medium bg-green-400 px-8 mx-10 my-1 rounded">
            Login
          </button>
          <p>
            <button
              @click.prevent="
                this.showLogin = false;
                this.errors = [];
              "
              class="border text-black font-medium bg-gray-400 px-8 mx-10 my-1 rounded"
            >
              Back
            </button>
          </p>
        </div>
      </div>
    </form>
  </Section>
</template>
<script>
import Section from "./Section.vue";
export default {
  components: {
    Section,
  },
  data() {
    return {
      loginParams: {
        email: "",
        password: "",
      },
      errors: [],
    };
  },
  methods: {
    login() {
      axios
        .post("/sessions.json", this.loginParams)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.id);
          this.user = response.data;
          this.activities = this.user.activities;
          this.didItsFullList = this.user.did_its;
          this.didIts = this.user.did_its;
          this.sortByDate(this.didIts);
          this.sortByDate(this.didItsFullList);
          this.didIts = this.didIts.reverse().slice(0, this.didItsNumber);
          console.log("Login successful");
          console.log("User: ", this.user);
          this.loginParams = {};
          this.showLogin = false;
          this.isLoggedIn = true;
          this.errors = [];
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.loginParams = {};
        });
    },
  },
};
</script>
