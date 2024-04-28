<!-- <template>
  <div class="register-form">
    <div style="margin: 3%; padding: 3%; width: 50%">
      <h3 style="text-align: left">Register</h3>
      <br />
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          ><b-form-input
            id="input-first-name-register"
            v-model="form.first_name"
            type="text"
            placeholder="Enter first name"
            :state="check_name"
            aria-describedby="input-live-feedback-first-name"
            required
          ></b-form-input>
          <b-form-invalid-feedback id="input-live-feedback-first-name">
            Enter at least 3 letters of first name
          </b-form-invalid-feedback>
        </b-form-group>

        <b-form-group
          ><b-form-input
            id="input-last-name-register"
            v-model="form.last_name"
            type="text"
            placeholder="Enter last name (Optional)"
          ></b-form-input
        ></b-form-group>

        <b-form-group label="Select role:" v-slot="{ ariaDescribedby }">
          <b-form-radio-group
            id="radio-group-role-register"
            v-model="form.role"
            :options="role_options"
            :aria-describedby="ariaDescribedby"
            name="radio-group-role"
          ></b-form-radio-group>
        </b-form-group>

        <b-form-group
          ><b-form-input
            id="input-email-register"
            v-model="form.email"
            type="email"
            placeholder="Enter email"
            required
          ></b-form-input
        ></b-form-group>

        <b-form-group
          ><b-form-input
            id="input-password-register"
            v-model="form.password"
            placeholder="Enter password"
            type="password"
            :state="check_password"
            aria-describedby="input-live-feedback-password"
            required
          ></b-form-input>
          <b-form-invalid-feedback id="input-live-feedback-password">
            Password should contain letters A-Z a-z and numbers 0-9 only and should be atleast 4 and
            atmost 8 characters long.
          </b-form-invalid-feedback>
        </b-form-group>

        <b-form-group
          ><b-form-input
            id="input-retype-password-register"
            v-model="form.retype_password"
            placeholder="Retype password"
            type="password"
            :state="check_retype_password"
            aria-describedby="input-live-feedback-retype-password"
            required
          ></b-form-input>
          <b-form-invalid-feedback id="input-live-feedback-retype-password">
            Password did not match.
          </b-form-invalid-feedback>
        </b-form-group>
        <br />
        <b-button style="margin: 10px" type="submit" variant="primary">Submit</b-button>
        <b-button style="margin: 10px" type="reset" variant="danger">Reset</b-button>
      </b-form>
      <br />
      <p>Already registered? Please <b-link href="/login">Login here</b-link></p>
      <p>Go to <b-link href="/home">Home Page</b-link></p>
    </div>
  </div>
</template>

<script>
import * as common from "../assets/common.js";

export default {
  name: "RegisterView",
  components: {},
  data() {
    return {
      role_options: [
        { text: "Student", value: "student" },
        { text: "Support", value: "support" },
        { text: "Admin", value: "admin" },
      ],
      form: {
        first_name: "",
        last_name: "",
        role: "student",
        email: "",
        password: "",
        retype_password: "",
      },
      show: true,
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      alert('You are creating a new account. Click "Ok" to proceed?');
      this.$log.info("Submitting Registration form");

      this.form.password = btoa(this.form.password);
      this.form.retype_password = btoa(this.form.retype_password);

      fetch(common.AUTH_API_REGISTER, {
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
              message: data.message,
            });
            this.$router.push("/login");
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
      this.form.first_name = "";
      this.form.last_name = "";
      this.form.email = "";
      this.form.password = "";
      this.form.retype_password = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
  computed: {
    check_name() {
      return this.form.first_name.length > 2 ? true : false;
    },
    check_password() {
      let password = this.form.password;
      if (password.length < 4 || password.length > 9) {
        return false;
      }
      const valid_char_array = Array.from(
        "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
      );
      const password_array = Array.from(password);
      for (let i = 0; i < password_array.length; i++) {
        if (!valid_char_array.includes(password_array[i])) {
          return false;
        }
      }
      return true;
    },
    check_retype_password() {
      return this.form.password === this.form.retype_password && this.check_password ? true : false;
    },
  },
};
</script> -->
<template>

  <div class="registration-page">
    <div class="form-header">
      <img src="../assets/logo.jpg" alt="Company Logo" class="company-logo" />
      <h2 class="form-title">Hello, Amigos</h2>
    </div>
    <div class="registration-form">
      <form class="form-fields" @submit="onSubmit" @reset="onReset" v-if="show" >

        <div class="inner-container">
          <div class="inner2-container">
            <label for="firstName" class="field-label">First Name</label>
            <input type="text"   v-model="form.first_name" id="firstName" placeholder="Enter your First Name" class="field-input" />
          </div>

          <div class="inner2-container">
            <label for="lastName" class="field-label">Last Name</label>
            <input type="text" id="lastName"  v-model="form.last_name"  placeholder="Enter your Last Name" class="field-input" />
          </div>
        </div>

        <div class="inner-container">
          <div class="inner2-container">
            <label for="email" class="field-label">Email Address</label>
            <input type="email" id="email"   v-model="form.email" placeholder="Enter your email address" class="field-input" />
          </div>

          <div class="inner2-container">
            <label for="StudentID" class="field-label">Student ID</label>
            <input type="text" id="studentId"  placeholder="Enter your ID" class="field-input" />
          </div>

        </div>

        <div class="inner-container">
          <div class="inner2-container">
            <label for="password" class="field-label">Password</label>
            <div class="password-field">
              <input type="password" id="password"  v-model="form.password" placeholder="Enter your password" class="extra-stuff" />
              <img src="../assets/hide.png"    alt=" Password visibility toggle" class="password-toggle" />
            </div>
          </div>

          <div class="inner2-container">
            <label for="confirmPassword" class="field-label">Re-Type Password</label>
            <div class="password-field">
              <input type="password" id="confirmPassword" v-model="form.retype_password"  placeholder="Re-Enter your password" class="extra-stuff" />
              <img src="../assets/hide.png" alt="Password Visibility Toggle" class="password-toggle" />
            </div>
          </div>

        </div>

        <div class="inner-container">
          <div class="inner2-container">
            <label for="role" class="field-label ap">Select Role</label>
            <div class="user-roles">
              <div class="role-option">
                <div class="role-radio">
                  <div class="role-radio-inner"></div>
                </div>
                <span class="role-label">Admin</span>
              </div>
              <div class="role-option">
                <div class="role-radio"></div>
                <span class="role-label">Support</span>
              </div>
              <div class="role-option">
                <div class="role-radio"></div>
                <span class="role-label">Student</span>
              </div>
            </div>

          </div>
          <div class="inner2-container"></div>
        </div>

        <div class="inner-container">
          <div class="inner2-container">
            <button type="submit" class="create-account-btn">Submit </button>
          </div>

          <div class="inner2-container">
            <button type="reset" class="create-account-btn " style="background-color: #3505BA;">Clear </button>
          </div>

        </div>

        <p class="login-link">
          Already have an account?
          <b-link href="/login" class="login-link-text">Login</b-link>
        </p>

        <p class="login-link">
          Visit Home?
          <b-link href="/home" class="login-link-text">Home</b-link>
        </p>


      </form>


    </div>

  </div>
</template>


<script>
import * as common from "../assets/common.js";

export default {
  name: "RegisterView",
  components: {},
  data() {
    return {
      role_options: [
        { text: "Student", value: "student" },
        { text: "Support", value: "support" },
        { text: "Admin", value: "admin" },
      ],
      form: {
        first_name: "",
        last_name: "",
        role: "student",
        email: "",
        password: "",
        retype_password: "",
      },
      show: true,
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      alert('You are creating a new account. Click "Ok" to proceed?');
      this.$log.info("Submitting Registration form");

      this.form.password = btoa(this.form.password);
      this.form.retype_password = btoa(this.form.retype_password);

      fetch(common.AUTH_API_REGISTER, {
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
              message: data.message,
            });
            this.$router.push("/login");
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
      this.form.first_name = "";
      this.form.last_name = "";
      this.form.email = "";
      this.form.password = "";
      this.form.retype_password = "";
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
  computed: {
    check_name() {
      return this.form.first_name.length > 2 ? true : false;
    },
    check_password() {
      let password = this.form.password;
      if (password.length < 4 || password.length > 9) {
        return false;
      }
      const valid_char_array = Array.from(
        "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
      );
      const password_array = Array.from(password);
      for (let i = 0; i < password_array.length; i++) {
        if (!valid_char_array.includes(password_array[i])) {
          return false;
        }
      }
      return true;
    },
    check_retype_password() {
      return this.form.password === this.form.retype_password && this.check_password ? true : false;
    },
  },
};
</script> 


<style>
.registration-page {
  background-color: var(--White, #fff);
  height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 30px 0px;
}

.inner3-container {
  display: flex;
  justify-content: space-between;
  width: inherit;

}

@media (max-width: 991px) {
  .registration-page {
    padding: 40px 20px;
  }
}

.registration-form {
  width: 90%;
  height: 80%;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
}

@media (max-width: 991px) {
  .registration-form {
    width: 100%;
  }
}

.ap {
  margin-top: 18px;
}

.inner-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 80%;
}

.inner2-container {
  width: 45%;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
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
  color: var(--G900, #100f14);
  font: 600 32px/128% Poppins, sans-serif;
  margin-top: 22px;
  text-align: center;
}

.form-fields {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}

.field-label {
  color: var(--G400, #9794aa);
  font: 500 16px/156% Poppins, sans-serif;
  width: 60%;
}

.field-input {
  border: 1px solid rgba(203, 202, 215, 1);
  border-radius: 6px;
  color: var(--G500, #686677);
  font: 500 14px/156% Poppins, sans-serif;
  margin-bottom: 22px;
  padding: 10px;
  width: auto;
  outline: none;
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
}

.password-toggle {
  width: 24px;
  margin-left: 8px;
  cursor: pointer;
}

.user-roles {
  display: flex;
  justify-content: space-between;
  margin-top: 5px;
}

.role-option {
  display: flex;
  align-items: center;
  gap: 11px;
}

.role-radio {
  border: 1px solid rgba(11, 135, 172, 1);
  border-radius: 32px;
  padding: 4px;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.role-radio-inner {
  background-color: var(--primary-blue-700, #0b87ac);
  border-radius: 50%;
  width: 12px;
  height: 12px;
}

.role-label {
  color: var(--G400, #9794aa);
  font: 500 16px/156% Poppins, sans-serif;
}

.create-account-btn {
  background-color: #ffaf1b;
  border: none;
  border-radius: 40px;
  color: var(--White, #fff);
  cursor: pointer;
  font: 500 20px Poppins, sans-serif;
  margin-top: 21px;
  padding: 18px 60px;
  text-align: center;
  display: block;
}

.login-link {
  color: var(--600, #6938ef);
  font: 400 22px/25px Poppins, sans-serif;
  margin-top: 28px;
  text-align: center;
}

.login-link-text {
  color: var(--600, #6938ef);
  font-weight: 600;
  text-decoration: underline;
}

.registration-image {
  width: 57%;
}
</style>