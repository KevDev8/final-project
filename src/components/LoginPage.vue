<template>
    <div class="login-wrapper row justify-content-center"> <!-- Centers the form horizontally on the page -->
        <div class="col-6"> <!-- Keeps the login at a reasonable and responsive width -->
            <div class="card p-4"> <!-- Adds padding to the form -->
                <h3 class="mb-3">Login</h3> <!-- Adds some extra margin at the bottom of the title -->
                <form @submit.prevent="submit"> <!-- Calls the submit method without reloading the page again when the submit button is clicked -->
                    <div class="mb-3"> <!-- More extra margin at the bottom to separate from the submit button -->
                        <label class="form-label">Password</label>
                        <input v-model="localPassword" type="password" class="form-control"/> <!-- Binds the data on the password field to the localPassword variable,
                                                                                                   styles slightly with form-control and hides the text with type="password" -->
                        <div class="form-text">Default password provided: <strong>cardinals</strong></div> <!-- Hints the default password -->
                    </div>

                    <div class="d-flex gap-2">
                        <button class="btn btn-primary" type="submit">Submit</button> <!-- Styles as primary button and defines as submit for the form -->
                    </div>

                    <div v-if="error" class="alert alert-danger mt-3">{{ error }}</div> <!-- Only shows up as a warning if the password doesn't match with cardinals, with some top margin from the rest of the form -->
                </form>
            </div>
        </div>
    </div>
</template>


<script>
export default {
    name: 'LoginPage',
    props: { password: { type: String, default: 'cardinals' } }, // Inherits the password passed from App.vue and defaults to cardinals if it fails to receive it
    data() {
        return {
            localPassword: this.password,
            error: null
        }
    },
    methods: {
        async submit() {
            this.error = null

            if (this.localPassword !== this.password) {
                this.error = 'Incorrect password.'
                return
            } // Verifies the password on the input correctly set to cardinals by default, raising an error if they don't match

            this.$emit('login-success') // Emits success so parent shows the data grid
        }
    }
}
</script>
