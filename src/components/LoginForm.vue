<template>
  <v-card class="d-flex flex-sm-column text-center pa-6 rounded-xl">
    <v-form>
      <v-text-field label="Email" color="success" v-model="email"> </v-text-field>
      <v-text-field
        label="Password"
        type="password"
        v-model="password"
        color="success"
      ></v-text-field>
      <v-alert transition="scale-transition" :value="wrongCredentials" color="red" dense>
        Wrong email/password
      </v-alert>
      <v-btn color="accent" block @click="login()">Login! </v-btn>
      <div class="ma-5">
        <a href="/account/restore"> Forgot your password? </a>
      </div>
      <hr />
      <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on, attrs }">
          <v-btn class="mt-6" color="success" dark v-bind="attrs" v-on="on">
            Create new account!
          </v-btn>
        </template>
        <register v-on:closeModal="dialog = false" />
      </v-dialog>
    </v-form>
  </v-card>
</template>

<script>
import Register from '@/components/RegisterForm';

export default {
  name: 'login',
  components: {
    Register,
  },
  data() {
    return {
      dialog: false,
      email: '',
      password: '',
      wrongCredentials: false,
    };
  },
  methods: {
    async login() {
      try {
        this.wrongCredentials = false;
        const { email } = this;
        const { password } = this;
        await this.$store.dispatch('login', { email, password });
        await this.$router.push(`/id${this.$store.state.id}`);
      } catch (e) {
        this.wrongCredentials = true;
      }
    },
  },
};
</script>
