<template>
  <div class="jumbotron">
    <div class="container">
      <div class="row">
        <div class="col-lg-8 offset-lg-2">
          <div>
            <div
              class="alert alert-danger"
              v-if="errors.length">
              <b>Please correct the following error(s):</b>
              <ul>
                <li
                  v-for="error in errors"
                  :key="error">{{ error }}</li>
              </ul>
            </div>
            <h2>Add New Route</h2>
            <form @submit.prevent="handleSubmit">
              <div class="form-group">
                <label>Date:</label>
                <input
                  type="date"
                  v-model="route.date"
                  name="date"
                  class="form-control"
                  :class="{ 'is-invalid': isSubmitted && !route.date }"
                >
              </div>
              <div class="form-group">
                <label>Description:</label>
                <input
                  type="text"
                  v-model="route.description"
                  name="description"
                  class="form-control"
                  :class="{ 'is-invalid': isSubmitted && !route.description }"
                >
              </div>
              <div class="form-group">
                <label>From:</label>
                <input
                  type="text"
                  v-model="route.from"
                  name="from"
                  class="form-control"
                  :class="{ 'is-invalid': isSubmitted && !route.from }"
                >
              </div>
              <div class="form-group">
                <label>Destination:</label>
                <input
                  type="text"
                  v-model="route.destination"
                  name="destination"
                  class="form-control"
                  :class="{ 'is-invalid': isSubmitted && !route.destination }"
                >
              </div>
              <div class="row">
                <div class="form-group col-sm-6">
                  <label>Starting Mileage:</label>
                  <input
                    type="number"
                    v-model="route.startMileage"
                    name="startMileage"
                    class="form-control"
                    :class="{ 'is-invalid': isSubmitted && !route.startMileage }"
                  >
                </div>
                <div class="form-group col-sm-6">
                  <label>Ending Mileage:</label>
                  <input
                    type="number"
                    v-model="route.endMileage"
                    name="endMileage"
                    class="form-control"
                    :class="{ 'is-invalid': isSubmitted && !route.endMileage }"
                  >
                </div>
              </div>
              <div class="form-group">
                <button
                  class="btn btn-primary"
                >Submit</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import uuidv4 from 'uuid/v4';
import * as actions from '../store/actions';

export default {
  name: 'RouteFormView',
  data() {
    return {
      route: {
        id: '',
        date: '',
        description: '',
        from: '',
        destination: '',
        startMileage: '',
        endMileage: '',
      },
      errors: [],
      isSubmitted: false,
    };
  },
  methods: {
    ...mapActions([actions.SUBMIT]),

    handleSubmit() {
      this.validateForm();
      this.isSubmitted = true;

      if (!this.errors.length) {
        this.route.id = uuidv4();
        this[actions.SUBMIT](this.route);
      }
    },

    validateForm() {
      this.errors = [];

      Object.keys(this.route)
        .filter(key => key !== 'id')
        .forEach((key) => {
          if (!this.route[key]) {
            this.errors.push(`${key} is required`);
          }
        });
    },
  },
};
</script>

<style scoped lang="scss">
  @import "../scss/base";

  .jumbotron {
    @include my(4);
  }
</style>

