<!-- <template>
  <div class="login-form">
    <div style="margin: 3%; padding: 3%; width: 50%">
      <h3 style="text-align: left">Login</h3>
      <br />
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          ><b-form-input
            id="input-email-login"
            v-model="form.email"
            type="email"
            placeholder="Enter email"
            required
          ></b-form-input
        ></b-form-group>

        <b-form-group
          ><b-form-input
            id="input-password-login"
            v-model="form.password"
            placeholder="Enter password"
            type="password"
            required
          ></b-form-input
        ></b-form-group>
        <br />
        <b-button style="margin: 10px" type="submit" variant="primary">Submit</b-button>
        <b-button style="margin: 10px" type="reset" variant="danger">Reset</b-button>
      </b-form>
      <br />

      <p>New user? Please <b-link href="/register">Register here</b-link></p>
      <p>Go to <b-link href="/home">Home Page</b-link></p>
    </div>
  </div>
</template>

<script>
import * as common from "../assets/common.js";

export default {
  name: "LoginView",
  components: {},
  data() {
    return {
      form: {
        email: "",
        password: "",
      },
      show: true,
    };
  },
  methods: {
    onSubmit: async function (event) {
      event.preventDefault();
      this.$log.info("Submitting Login form");
      this.form.password = btoa(this.form.password);

      fetch(common.AUTH_API_LOGIN, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(this.form),
      })
        .then((response) => response.json())
        .then((data) => {
          if (data.category == "success") {
            this.flashMessage.success({
              message: "Successfully logged in.",
            });

            // update store
            this.$store.dispatch("set_state_after_login", data.message);
            this.$store.dispatch("token_timeout_fn", {});
            this.$router.push(`/${data.message.role}-home`); //home page depends on role
          }
          if (data.category == "error") {
            this.flashMessage.error({
              message: data.message,
            });
          }
        })
        .catch((error) => {
          this.$log.error(`Error : ${error}`);
          this.flashMessage.error({
            message: "Internal Server Error",
          });
        });
    },
    onReset(event) {
      event.preventDefault();
      this.form.email = "";
      this.form.password = "";
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
};
</script>

<style></style> -->
<template>
  <div class="login-page">
    <div class="form-column">
      <div class="form-header">
        <img src="../assets/logo.jpg" alt="Company Logo" class="company-logo" />
        <h2 class="form-title">Welcome Back, Amigos</h2>
      </div>
      <form class="login-form">
        <label for="email" class="field-label">Email Address</label>
        <input type="email" id="email" placeholder="Enter your email address" class="field-input" />
        <label for="password" class="field-label">Password</label>

        <div class="password-field">
          <input type="password" id="password" placeholder="Enter your password" class="extra-stuff" />
          <img src="../assets/hide.png" alt=" Password visibility toggle" class="password-toggle" />
        </div>
        <button type="submit" class="login-button">Log In</button>
      </form>

      <p class="login-link">
        Don't have an Account?
        <b-link href="/register"  class="login-link-text">SignUp</b-link>
      </p>


    </div>

  </div>
</template>

<style scoped>
.login-page {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 100px;
  background-color: #fff;
  height: 100vh;
  width: 100%;
  background-image: url('../assets/background2.jpg');
  background-size:cover;
}



.login-link {
  color: var(--600, white);
  font: 400 22px/25px Poppins, sans-serif;
  margin-top: 28px;
  text-align: center;
}

.login-link-text {
  color: var(--600, white);
  font-weight: 600;
  text-decoration: underline;
}

.field-label {
  color: var(--G400,white);
  font: 500 16px/156% Poppins, sans-serif;

}

.field-input {
  border: 1px solid rgba(203, 202, 215, 1);
  border-radius: 6px;
  color: var(--G500, #686677);
  font: 500 14px/156% Poppins, sans-serif;
  margin-bottom: 22px;
  padding: 10px;
  width: inherit;
  outline: none;
}

@media (max-width: 991px) {
  .login-container {
    flex-direction: column;
    align-items: stretch;
    gap: 0;
  }
}

.image-column {
  width: 50%;
  margin-left: 0;
}

.form-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px;
}

.company-logo {
  width: 200px;
  border-radius: 6px;
}

.form-title {
  color: var(--G900, white);
  font: 600 32px/128% Poppins, sans-serif;
  margin-top: 22px;
  text-align: center;
}

@media (max-width: 991px) {
  .image-column {
    width: 100%;
  }
}

.login-image {
  aspect-ratio: 0.83;
  object-fit: cover;
  height: inherit;
  width: inherit;
}

@media (max-width: 991px) {
  .login-image {
    max-width: 100%;
    margin-top: 39px;
  }
}

.form-column {
  width: 50%;
  padding: 50px 100px;
  margin-right: 20px;
}

@media (max-width: 991px) {
  .form-column {
    width: 100%;
  }
}

.form-wrapper {
  display: flex;
  flex-direction: column;
  align-self: stretch;
  font-size: 16px;
  font-weight: 500;
  line-height: 156%;
  margin: auto 0;
  padding: 0 20px;
}

@media (max-width: 991px) {
  .form-wrapper {
    max-width: 100%;
    margin-top: 40px;
  }
}

.extra-stuff {
  color: var(--G500, #686677);
  font: 500 14px/156% Poppins, sans-serif;
  border: none;
  outline: none;
  width: 100%;
}

.password-field {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid rgba(203, 202, 215, 1);
  border-radius: 6px;
  margin-top: 6px;
  padding: 10px;
  background-color: white;
}






.login-form {
  display: flex;
  flex-direction: column;
}






.login-button {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #3505ba;
  color: #fff;
  border: none;
  border-radius: 40px;
  padding: 18px 60px;
  margin-top: 64px;
  font-family: Poppins, sans-serif;
  font-size: 20px;
  text-align: center;
  cursor: pointer;
}

@media (max-width: 991px) {
  .login-button {
    max-width: 100%;
    margin-top: 40px;
    padding: 18px 20px;
  }
}

.signup-link {
  color: #6938ef;
  text-align: center;
  letter-spacing: 0.11px;
  text-decoration: none;
  align-self: center;
  margin-top: 50px;
  font: 400 22px/25px Poppins, sans-serif;
}

@media (max-width: 991px) {
  .signup-link {
    max-width: 100%;
    margin-top: 40px;
  }
}

.signup-link-text {
  font-weight: 600;
  text-decoration: underline;
  color: #6938ef;
}
</style>