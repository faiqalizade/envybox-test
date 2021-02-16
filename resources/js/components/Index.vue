<template>
  <div>
    <form novalidate class="md-layout" method="post">
    <input type="hidden" name="_token" :value="csrf">
      <md-card class="md-layout-item md-size-50 md-small-size-100">
        <md-card-header>
          <div class="md-title">Orders</div>
        </md-card-header>

        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('firstName')">
                <label for="first-name">First Name</label>
                <md-input name="name" id="first-name" autocomplete="given-name" v-model="form.firstName" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.firstName.required">The first name is required</span>
                <span class="md-error" v-else-if="!$v.form.firstName.minlength">Invalid first name</span>
              </md-field>
            </div>
          </div>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field :class="getValidationClass('firstName')">
                <label for="first-name">Phone</label>
                <md-input name="phone" id="first-name" autocomplete="given-name" v-model="form.phone" :disabled="sending" />
                <span class="md-error" v-if="!$v.form.phone.required">The first name is required</span>
                <span class="md-error" v-else-if="!$v.form.phone.minlength">Invalid first name</span>
              </md-field>
            </div>
          </div>
          <md-field>
            <label>Message</label>
            <md-textarea name="message" v-model="message" md-autogrow></md-textarea>
        </md-field>
        </md-card-content>

        <md-progress-bar md-mode="indeterminate" v-if="sending" />

        <md-card-actions>
            <md-button type="submit" class="md-primary" :disabled="sending">Send Order</md-button>
            </md-card-actions>
        </md-card>

      <md-snackbar :md-active.sync="userSaved">The user {{ lastUser }} was saved with success!</md-snackbar>
    </form>
  </div>
</template>

<script>
    import Vue from 'vue'
    import VueMaterial from 'vue-material'
    import 'vue-material/dist/vue-material.min.css'
    import 'vue-material/dist/theme/default.css'

    Vue.use(VueMaterial)
    import {
        validationMixin
    } from 'vuelidate'
    import {
        required,
        email,
        minLength,
        maxLength,
    } from 'vuelidate/lib/validators'

    export default {
        name: "Index",
        mixins: [validationMixin],
        data: () => ({
            form: {
                firstName: null,
                phone: null,
                email: null,
            },
            userSaved: false,
            sending: false,
            lastUser: null,
            message: '',
            csrf: document.querySelector('meta[name="csrf-token"]').getAttribute('content')
        }),
        validations: {
            form: {
                firstName: {
                    required,
                    minLength: minLength(3)
                },
                phone:{
                    required,
                },
                email: {
                    required,
                    email
                }
            }
        },
        methods: {
            getValidationClass(fieldName) {
                const field = this.$v.form[fieldName]

                if (field) {
                    return {
                        'md-invalid': field.$invalid && field.$dirty
                    }
                }
            },
            clearForm() {
                this.$v.$reset()
                this.form.firstName = null
                this.form.lastName = null
                this.form.age = null
                this.form.gender = null
                this.form.email = null
            },
            saveUser() {
                this.sending = true

                // Instead of this timeout, here you can call your API
                window.setTimeout(() => {
                    this.lastUser = `${this.form.firstName} ${this.form.lastName}`
                    this.userSaved = true
                    this.sending = false
                    this.clearForm()
                }, 1500)
            },
            validateUser() {
                this.$v.$touch()

                if (!this.$v.$invalid) {
                    this.saveUser()
                }
            }
        }
    }
</script>

<style lang="scss">
    @import '/css/app.css';
  .md-progress-bar {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
  }
</style>