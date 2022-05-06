<template>
  <v-form>
    <br />
    <h2>User Form</h2>

    <v-text-field v-if="id" v-model="id" label="Id"> </v-text-field>
    <v-text-field v-model="name" label="Name" hint="Enter your full name">
    </v-text-field>
    <v-text-field
      v-model="email"
      label="Email"
      hint="Enter valid email address"
    >
    </v-text-field>
    <v-text-field
      v-model="password"
      :append-icon="show3 ? 'mdi-eye' : 'mdi-eye-off'"
      :type="show3 ? 'text' : 'password'"
      name="input-10-2"
      label="Password"
      hint="At least 8 characters"
      value=""
      class="input-group--focused"
      @click:append="show3 = !show3"
    ></v-text-field>

    <v-btn class="blue--text" @click="submit({ id, name, email, password })">{{
      id ? "Edit" : "Submit"
    }}</v-btn>
  </v-form>
</template>

<script>
export default {
  computed: {
    id: {
      get() {
        return this.$store.state.user.id;
      },
      set(value) {
        this.$store.commit("user/storeId", value);
      },
    },
    name: {
      get() {
        return this.$store.state.user.name;
      },
      set(value) {
        this.$store.commit("user/storeName", value);
      },
    },
    email: {
      get() {
        return this.$store.state.user.email;
      },
      set(value) {
        this.$store.commit("user/storeEmail", value);
      },
    },
    password: {
      get() {
        return this.$store.state.user.password;
      },
      set(value) {
        this.$store.commit("user/storePassword", value);
      },
    },
  },

  data() {
    return {
      show1: false,
      show2: true,
      show3: false,
      show4: false,
    };
  },

  methods: {
    async submit(user) {
      if (user.id) {
        await this.$axios.put(
          "http://localhost:8081/user/update/" + user.id,
          user
        );
      } else {
        await this.$axios.post("http://localhost:8081/user/create/", user);
      }
      this.restForm({ id: 0, name: "", email: "", password: "" });
      this.$store.commit(
        "users/storeData",
        (await this.$axios.get("http://localhost:8081/user/get/")).data
      );
    },
    restForm(user) {
      this.$store.commit("user/storeId", user.id);
      this.$store.commit("user/storeName", user.name);
      this.$store.commit("user/storeEmail", user.email);
      this.$store.commit("user/storePassword", user.password);
    },
  },
};
</script>

<style></style>
