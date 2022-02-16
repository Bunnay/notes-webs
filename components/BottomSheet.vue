<template>
  <div class="text-center">
    <v-fab-transition>
      <v-btn color="teal" dark fixed bottom right fab @click="sheet = true">
        <v-icon>mdi-login</v-icon>
      </v-btn>
    </v-fab-transition>
    <v-bottom-sheet v-model="sheet" persistent>
      <v-sheet>
        <v-container>
          <h3 class="mb-4 mt-2">Login</h3>
          <v-text-field
            v-model="login.identifier"
            label="Username"
            filled
            class="rounded w-50"
          >
          </v-text-field>
          <v-text-field
            v-model="login.password"
            label="Password"
            filled
            class="rounded w-50"
          >
          </v-text-field>
          <div class="d-flex">
            <v-btn color="red" @click="sheet = !sheet" outlined> close </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="userLogin" outlined :loading="loading">
              Submit
            </v-btn>
          </div>
        </v-container>
      </v-sheet>
    </v-bottom-sheet>
    <Snackbar
      :color="success ? 'primary' : 'red'"
      :snackbar="snackbar"
      :text="success ? success : error"
      :icon="success ? 'mdi-user' : 'mdi-user'"
    ></Snackbar>
  </div>
</template>

<script>
import Snackbar from "./Snackbar.vue";
export default {
  components: { Snackbar },
  data() {
    return {
      sheet: false,
      login: {
        identifier: '',
        password: '',
      },
      error: null,
      success: null,
      loading: false,
      dialog: false,
      snackbar: false,
    };
  },
  methods: {
    async userLogin() {
        this.error = null;
        this.success = null;
        this.loading = true;
        try {
          await this.$auth.loginWith('local', {
            data : this.login
          });
          this.dialog = false;
          this.success = "Successfully LoggedIn!";
        }
        catch(err) {
          this.error = "Incorrect Password or Username";
        }
        finally {
          this.loading = false;
          this.snackbar = true;
        };
        setTimeout(() => {
          this.snackbar = false;
        }, 1000);
      },
  }
};
</script>

<style lang="scss" scoped>
::v-deep .v-input__slot::before {
  border-style: none !important;
}
</style>
