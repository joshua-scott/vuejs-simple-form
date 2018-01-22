<template>
    <div class="container">
        <form 
            v-if="!submitted" 
            @submit.prevent="submitForm">
            <div class="row">
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                    <img :src="'https://raw.githubusercontent.com/joshua-scott/vuejs-simple-form/master/dist/logo.png?raw=true'" alt="Vue.js logo" class="img-responsive center-block">
                    <h2 class="title">An example form in Vue.js</h2>
                    <p class="notice"><em>Note: HTML 'require' attributes should also be used, but are omitted here in favour of learning Vue. Ditto for 'type="email"', and the minlength password attribute</em></p>
                    <div class="form-group">
                        <label for="first-name">First name</label>
                        <input
                            type="text" 
                            class="form-control" 
                            id="first-name" 
                            placeholder="John"
                            v-model.trim="user.firstName">
                    </div>
                    <div class="form-group">
                        <label for="last-name">Last name</label>
                        <input
                            type="text" 
                            class="form-control" 
                            id="last-name" 
                            placeholder="Smith"
                            v-model.trim="user.lastName">
                    </div>
                    <div class="form-group">
                        <label for="email">Email address</label>
                        <input
                            type="email" 
                            class="form-control" 
                            id="email" 
                            placeholder="johnsmith@example.com"
                            v-model.trim="user.email">
                        <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
                    </div>
                    <div class="form-group">
                        <label for="password1">Password (minimum {{ minPasswordLength }} characters)</label>
                        <input
                            type="password" 
                            class="form-control" 
                            id="password1" 
                            placeholder="Your secure password"
                            v-model="user.password1">
                    </div>
                    <div class="form-group">
                        <label for="password2">Password (again)</label>
                        <input
                            type="password" 
                            class="form-control" 
                            id="password2" 
                            placeholder="Retype password"
                            v-model="user.password2">
                    </div>
                    <div class="form-check">
                        <button class="btn btn-success" type="submit">Submit</button>
                        <label class="form-check-label store-data-label" for="data-check">Store data?</label>
                        <input
                            type="checkbox" 
                            class="form-check-input" 
                            id="data-check"
                            v-model="user.storeData">
                    </div>
                </div>
            </div>
        </form>
        <div class="row" v-if="submitted">
            <hr>
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <div class="panel panel-default">
                    <div class="panel-heading">
                        <h4>Thanks, here's your data:</h4>
                    </div>
                    <div class="panel-body">
                        <p>Full Name: {{ fullname }}</p>
                        <p>Mail: {{ user.email }}</p>

                        <input 
                            type="checkbox" 
                            class="form-check-input" 
                            id="reveal-password"
                            v-model="revealPassword">
                        <label for="reveal-password" style="font-weight: 400;">Reveal password?</label>
                        <span v-if="revealPassword">
                            <span v-if="!passwordsMatch()"><em>{{ errorMsg.differentPasswords }}</em></span>
                            <span v-else-if="!passwordLength()"><em>{{ errorMsg.passwordTooShort }}</em></span>
                            <span v-else>{{ `"${user.password1}"` }}</span>
                        </span>

                        <p>Store in Database?: {{ user.storeData ? 'Yes' : 'No' }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                user: {
                    firstName: '',
                    lastName: '',
                    email: '',
                    password1: '',
                    password2: '',
                    storeData: true
                },
                minPasswordLength: 8,
                revealPassword: false,
                submitted: false
            }
        },
        computed: {
            fullname() {
                return `${this.user.firstName} ${this.user.lastName}`
            },
            errorMsg() {
                return {
                    differentPasswords: `Passwords don't match`,
                    passwordTooShort: `${this.minPasswordLength} characters minimum required`
                }
            }
        },
        methods: {
            passwordsMatch() {
                return this.user.password1 === this.user.password2
            },
            passwordLength() {
                return this.user.password1.length >= 8
            },
            submitForm() {
                // check for errors
                if (this.user.firstName  == '' ||
                    this.user.lastName   == '' ||
                    this.user.email      == '' ||
                    this.user.password1  == ''  ) {
                        return alert('Please complete all fields.')
                }  else if (!this.passwordsMatch()) {
                    return alert(this.errorMsg.differentPasswords)
                } else if (!this.passwordLength()) {
                    return alert(this.errorMsg.passwordTooShort)
                }
                
                if (this.storeData) {
                   // logic to actually store data would be here
                }

                this.submitted = true
            }
        }
    }
</script>

<style>
.title {
    text-align: center;
}

.notice {
    padding: 15px 5px;
}

.store-data-label {
    margin-left: 20px;
}
</style>