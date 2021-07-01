<!--
 * @Author: hhhhhq
 * @Date: 2021-07-01 16:22:31
 * @LastEditors: hhhhhq
 * @LastEditTime: 2021-07-01 22:51:56
 * @Description: file content
-->
<template>
  <div class="root">
    <h2>Create an Account</h2>
    <p>
      <input type="text" placeholder="email" v-model="state.email" />
      <span v-if="v$.email.$error">
        {{ v$.email.$errors[0].$message }}
      </span>
    </p>
    <p>
      <input
        type="password"
        placeholder="password"
        v-model="state.password.password"
      />
      <span v-if="v$.password.password.$error">
        {{ v$.password.password.$errors[0].$message }}
      </span>
    </p>
    <p>
      <input
        type="password"
        placeholder="confirm password"
        v-model="state.password.confirm"
      />
      <span v-if="v$.password.confirm.$error">
        {{ v$.password.confirm.$errors[0].$message }}
      </span>
    </p>
    <button @click="submitForm">Submit</button>
  </div>
</template>

<script>
import { reactive, computed } from "vue"
import useValidate from "@vuelidate/core"
import {
  required,
  email,
  minLength,
  sameAs,
  helpers,
} from "@vuelidate/validators"
export default {
  setup() {
    const state = reactive({
      email: "",
      password: {
        password: "",
        confirm: "",
      },
    })

    const mustBeLearnVue = value => value.includes(`learnvue`)

    const rules = computed(() => {
      return {
        email: {
          required,
          email,
          mustBeLearnVue: helpers.withMessage(
            `必须包含 learnvue`,
            mustBeLearnVue
          ),
        },
        password: {
          password: { required, minLength: minLength(6) },
          confirm: { required, sameAs: sameAs(state.password.password) },
        },
      }
    })

    const v$ = useValidate(rules, state)

    return {
      state,
      v$,
    }
  },
  methods: {
    submitForm() {
      console.log(this.v$)
      this.v$.$validate()
      if (!this.v$.$error) {
        alert("Form successfully submitted.")
      } else {
        alert("Form failed validation")
      }
    },
  },
}
</script>

<style scoped>
.root {
  width: 400px;
  margin: 0 auto;
  background: #fff;
  padding: 30px;
  margin-top: 100px;
  border-radius: 20px;
}

input {
  width: 100%;
  border: none;
  outline: none;
  border-bottom: 1px solid #ddd;
  font-size: 1em;
  margin: 10px 0 5px 0;
}

button {
  padding: 10px 15px;
  color: #000;
  background-color: #2196f3;
  border: none;
}
</style>
