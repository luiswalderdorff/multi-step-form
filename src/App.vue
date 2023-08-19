<template>
  <div id="app">
    <form @submit.prevent="submitForm">
      <div v-if="step === 1">
        <h2>Your Info</h2>
        <label>Name: <input v-model="form.name" required /></label>
        <label
          >Email: <input v-model="form.email" type="email" required
        /></label>
        <label>Phone: <input v-model="form.phone" required /></label>
        <p v-if="errors.name">Name is required</p>
        <p v-if="errors.email">Email is required and must be valid</p>
        <p v-if="errors.phone">Phone number is required</p>
        <button type="button" @click="validateStep1">Next</button>
      </div>

      <div v-if="step === 2">
        <h2>Select Plan</h2>
        <label>
          <input type="radio" v-model="form.plan" value="arcade" required />
          Arcade {{ isYearly ? "$90/yr" : "$9/mo" }}
        </label>
        <label>
          <input type="radio" v-model="form.plan" value="advanced" required />
          Advanced {{ isYearly ? "$120/yr" : "$12/mo" }}
        </label>
        <label>
          <input type="radio" v-model="form.plan" value="pro" required />
          Pro {{ isYearly ? "$150/yr" : "$15/mo" }}
        </label>
        <label>
          <input type="checkbox" v-model="isYearly" />
          Yearly
        </label>
        <p v-if="errors.plan">You must select a plan</p>
        <button type="button" @click="prevStep">Previous</button>
        <button type="button" @click="validateStep2">Next</button>
      </div>

      <div v-if="step === 3">
        <h2>Add-Ons</h2>
        <label>
          <input type="checkbox" v-model="form.addOns" value="onlineService" />
          Online Service {{ isYearly ? "+$10/yr" : "+$1/mo" }}
        </label>
        <label>
          <input type="checkbox" v-model="form.addOns" value="largerStorage" />
          Larger Storage {{ isYearly ? "+$20/yr" : "+$2/mo" }}
        </label>
        <label>
          <input
            type="checkbox"
            v-model="form.addOns"
            value="customizableProfile"
          />
          Customizable Profile {{ isYearly ? "+$20/yr" : "+$2/mo" }}
        </label>
        <button type="button" @click="prevStep">Previous</button>
        <button type="button" @click="nextStep">Next</button>
      </div>

      <div v-if="step === 4">
        <h2>Summary</h2>
        <p>Plan: {{ form.plan }}</p>
        <p>Add-Ons: {{ form.addOns.join(", ") }}</p>
        <button type="button" @click="prevStep">Previous</button>
        <button type="submit">Submit</button>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

export default defineComponent({
  data() {
    return {
      step: 1,
      isYearly: false,
      form: {
        name: "",
        email: "",
        phone: "",
        plan: "",
        addOns: [],
      },
      errors: {
        name: false,
        email: false,
        phone: false,
        plan: false,
      },
    };
  },
  methods: {
    nextStep() {
      console.log("nextStep");

      this.step++;
    },
    prevStep() {
      this.step--;
    },
    validateStep1() {
      this.errors = {
        name: false,
        email: false,
        phone: false,
        plan: false,
      };

      console.log("validateStep1");
      if (!this.form.name) this.errors.name = true;
      if (!this.form.email || !/\S+@\S+\.\S+/.test(this.form.email))
        this.errors.email = true;
      if (!this.form.phone) this.errors.phone = true;
      if (Object.values(this.errors).every((value) => value === false))
        this.nextStep();
    },
    validateStep2() {
      this.errors = {
        name: false,
        email: false,
        phone: false,
        plan: false,
      };
      if (!this.form.plan) this.errors.plan = true;
      if (Object.values(this.errors).every((value) => value === false))
        this.nextStep();
    },
    submitForm() {
      console.log("submitForm");

      console.log(this.form);
    },
  },
});
</script>
