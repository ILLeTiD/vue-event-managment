<template>
  <div>
    <v-container>
        <v-layout row v-if="error">
        <v-flex xs12 sm6>
          <app-alert :text="error.message" @dismissed="onDismissed">
          </app-alert>
        </v-flex>
      </v-layout>
      <v-layout>
        <v-flex xs12 sm6 offset-sm3>
          <v-card>
            <v-card-text>
              <v-container>
                <form @submit.prevent="onSignin">
                  <v-layout row>
                    <v-flex xs12>
                      <v-text-field name="email" :rules=[rules.email] label="Mail" id="email" type="email" v-model="email" required>
                      </v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout row>
                    <v-flex xs12>
                      <v-text-field name="password" label="Password" id="password" type="password" v-model="password" required>
                      </v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout row>
                    <v-flex xs12>
                     <v-btn type="submit" :disabled="loading" :loading="loading">
                        Sign in
                         <span slot="loader" class="custom-loader">
                          <v-icon light>cached</v-icon>
                        </span>
                      </v-btn>
                    </v-flex>
                  </v-layout>
                </form>
              </v-container>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        email: '',
        password: '',
        rules: {
          email: (value) => {
            const pattern =
              /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
            return pattern.test(value) || 'Invalid e-mail.'
          }
        }
      }
    },
    computed: {
      user() {
        return this.$store.getters.user
      },
      error () {
        return this.$store.getters.error;
      }, 
      loading () {
        return this.$store.getters.loading;
      }
    },
    watch: {
      user(val) {
        if (val !== null && val !== undefined) {
          this.$router.push('/')
        }
      }
    },
    methods: {
      onSignin() {
        this.$store.dispatch('signUserIn', {
          email: this.email,
          password: this.password
        })
      },
       onDismissed () {
        console.log('dismissed alert!');
        this.$store.dispatch('clearError');
      }
    }
  }

</script>
