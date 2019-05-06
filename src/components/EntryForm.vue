<template>
  <div class="entry-form">
    <!--container with the data entry form -->
    <b-container class="bv-example-row mt-5 pt-5 mb-5 pb-5">
      <b-row>
        <b-col cols="2"></b-col>
        <b-col cols="8">

          <div role="tablist">
            <b-card no-body class="mb-1">
              <b-card-header header-tag="header" class="p-1" role="tab">
                <b-button block href="#" v-b-toggle.section1 variant="info" class="text-left">Step 1: Your details
                </b-button>
              </b-card-header>
              <b-collapse ref="section1" id="section1" visible accordion="my-accordion" role="tabpanel">
                <b-card-body>
                  <b-form ref="form1" @submit="moveToStep2">

                    <b-container class="bv-example-row">
                      <b-row>
                        <b-col>

                          <b-form-group id="input-group-1" label="First name:" label-for="firstName">
                            <b-form-input
                                    id="firstName"
                                    v-model="form.firstName"
                                    type="text"
                            >
                            </b-form-input>
                          </b-form-group>

                        </b-col>
                        <b-col>

                          <b-form-group id="input-group-2" label="Last name:" label-for="lastName">
                            <b-form-input
                                    id="lastName"
                                    v-model="form.lastName"
                                    type="text"
                            >
                            </b-form-input>
                          </b-form-group>

                        </b-col>
                        <div class="w-100"></div>
                        <b-col>

                          <b-form-group id="input-group-3" label="Email address:" label-for="email">
                            <b-form-input
                                    id="email"
                                    v-model="form.email"
                                    placeholder="yourname@example.com"
                                    type="text"
                            >
                            </b-form-input>
                          </b-form-group>

                        </b-col>
                        <b-col>

                          <b-form-group id="input-group-4" label="Mobile number:" label-for="mobile">
                            <b-form-input
                                    id="mobile"
                                    v-model="form.mobile"
                                    type="text">
                            </b-form-input>
                          </b-form-group>

                        </b-col>
                        <div class="w-100"></div>
                        <b-col>

                          <b-form-group id="input-group-5" label="Gender:">
                            <b-form-radio v-model="form.gender" name="gender-choice" value="Male">Male</b-form-radio>
                            <b-form-radio v-model="form.gender" name="gender-choice" value="Female">Female
                            </b-form-radio>
                            <b-form-radio v-model="form.gender" name="gender-choice" value="Other">Other</b-form-radio>
                          </b-form-group>

                        </b-col>
                        <b-col>

                          <b-form-group id="input-group-6" label="Date of birth:" label-for="dob">
                            <b-form-input
                                    id="dob"
                                    type="date"
                                    v-model="form.dob"
                                    placeholder="dd/mm/yyyy"
                            >
                            </b-form-input>
                          </b-form-group>

                        </b-col>
                      </b-row>
                    </b-container>

                    <div class="navbar">
                      <b-button class="w-25" id="next1" type="submit">Next ></b-button>
                    </div>

                    <div v-if="errors.length">
                      <b>Please correct the following error(s):</b>
                      <ul>
                        <li v-for="error in errors" v-bind:key="error.id">{{ error }}</li>
                      </ul>
                    </div>
                  </b-form>
                </b-card-body>
              </b-collapse>
            </b-card>

            <b-card no-body class="mb-1">
              <b-card-header header-tag="header" class="p-1" role="tab">
                <b-button block href="#" v-b-toggle.section2 variant="info" class="text-left">Step 2: More comments
                </b-button>
              </b-card-header>
              <b-collapse id="section2" accordion="my-accordion" role="tabpanel">
                <b-card-body>

                  <b-form ref="form2" @submit="moveToStep3">
                    <b-form-group id="input-group-4" label="Comments:" label-for="comments">
                      <b-form-textarea
                              id="comments"
                              v-model="form.comments">
                      </b-form-textarea>
                    </b-form-group>
                    <div class="navbar">
                      <b-button class="w-25" id="next2" v-on:click="moveToStep3">Next</b-button>
                    </div>
                  </b-form>
                </b-card-body>
              </b-collapse>
            </b-card>

            <b-card no-body class="mb-1">
              <b-card-header header-tag="header" class="p-1" role="tab">
                <b-button block href="#" v-b-toggle.section3 variant="info" class="text-left">Step 3: Final comments
                </b-button>
              </b-card-header>
              <b-collapse id="section3" accordion="my-accordion" role="tabpanel">
                <b-card-body>
                  <b-form ref="form3" @submit="save">
                    <b-form-group id="input-group-5" label="Final comments:" label-for="finalComments">
                      <b-form-textarea
                              id="finalComments"
                              v-model="form.finalComments">
                      </b-form-textarea>
                    </b-form-group>
                    <div class="navbar">
                      <b-button class="w-25" id="save" type="submit" variant="success">Save</b-button>
                    </div>

                    <div v-if="success">
                      <b>Your details were successfully saved.</b>
                    </div>

                    <div v-if="error">
                      <b>Your details were not saved.</b>
                      <b>{{ error }}</b>

                    </div>
                  </b-form>
                </b-card-body>
              </b-collapse>
            </b-card>
          </div>
        </b-col>
        <b-col cols="2"></b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>
    /* eslint-disable no-useless-escape */

    import axios from 'axios'

    export default {
        name: 'EntryForm',
// methods that toggle collapse of the form sections
        methods: {
            moveToStep2: function (event) {
                event.preventDefault();
                if (this.checkEntries()) {
                    this.$root.$emit('bv::toggle::collapse', 'section2');
                    this.errors = [];
                }
            },
            moveToStep3: function (event) {
                event.preventDefault();
                this.$root.$emit('bv::toggle::collapse', 'section3');
            },
            save: function (event) {
                event.preventDefault();
                if (!this.checkEntries()) {
                    this.$root.$emit('bv::toggle::collapse', 'section1');
                    return;
                }
                this.submitData();
            },
            // method to submit data to backend API
            submitData: function () {
                let component = this;
                axios.post('http://phplaravel-228259-831886.cloudwaysapps.com/api/store', {
                    firstName: this.form.firstName,
                    lastName: this.form.lastName,
                    email: this.form.email,
                    mobile: this.form.mobile,
                    gender: this.form.gender,
                    dob: this.form.dob,
                    comments: this.form.comments,
                    finalComments: this.form.finalComments
                })
                    .then(function () {
                        component.success = true;

                    })
                    .catch(function (error) {
                        component.error = error;
                    });
            },
            //validation of form entries
            checkEntries: function () {

                this.errors = [];

                if (!this.form.firstName) {
                    this.errors.push('First name required.');
                }
                if (!this.form.lastName) {
                    this.errors.push('Last name required.');
                }

                if (!this.form.email) {
                    this.errors.push('Email required.');
                }
                else {
                    if (!this.validEmail(this.form.email)) {
                        this.errors.push('Valid email required.');
                    }

                    if (!this.form.mobile) {
                        this.errors.push('Mobile number required.');
                    }
                    else {
                        if (!this.validMobile(this.form.mobile)) {
                            this.errors.push('Valid mobile required.');
                        }
                    }

                    if (!this.form.gender) {
                        this.errors.push('Gender required.');
                    }

                    if (!this.form.dob) {
                        this.errors.push('Date of birth required.');
                    }
                    return this.errors.length === 0;
                }
            },
            validMobile: function () {
                let checkMobile = /^(?:(?:\(?(?:0(?:0|11)\)?[\s-]?\(?|\+)44\)?[\s-]?(?:\(?0\)?[\s-]?)?)|(?:\(?0))(?:(?:\d{5}\)?[\s-]?\d{4,5})|(?:\d{4}\)?[\s-]?(?:\d{5}|\d{3}[\s-]?\d{3}))|(?:\d{3}\)?[\s-]?\d{3}[\s-]?\d{3,4})|(?:\d{2}\)?[\s-]?\d{4}[\s-]?\d{4}))(?:[\s-]?(?:x|ext\.?|\#)\d{3,4})?$/;
                return checkMobile.test(this.form.mobile);
            },

            validEmail: function () {
                let checkEmail = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
                return checkEmail.test(this.form.email);
            }
        },
        data() {
            return {
                errors: [],
                error: '',
                success: false,
                form: {
                    firstName: '',
                    lastName: '',
                    email: '',
                    mobile: '',
                    gender: '',
                    dob: '',
                    comments: '',
                    finalComments: '',
                }
            }
        }
    }
</script>

<style scoped>
  .btn-info {
    background-color: #ff0b68;
    border-color: #ff175b;
  }

  .btn-info:hover {
    background-color: #ef3c73;
    border-color: #ef3c73;
  }

  .btn-info:not(:disabled):not(.disabled):active {
    background-color: #a93864;
    border-color: #a93864;
  }

  .navbar {
    display: flex;
    justify-content: flex-end;
  }

  #next1, #next2, #save {
    border-radius: 50px;
  }

</style>

