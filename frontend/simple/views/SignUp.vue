<template>
  <section class="section">
    <!-- main containers:
     .container  http://bulma.io/documentation/layout/container/
     .content    http://bulma.io/documentation/elements/content/
     .section    http://bulma.io/documentation/layout/section/
     .block      base/classes.sass (just adds 20px margin-bottom except for last)
     -->
    <form v-el:form class="container signup">
      <div class="columns is-gapless">
        <div class="column is-hidden-mobile"></div>
        <div class="column is-10">
          <div class="box centered" style="max-width:400px">
            <h2 class="subtitle">Sign Up</h2>
                  
            <p class="control has-icon">
              <input class="input" name="name" placeholder="username">
              <i class="fa fa-user"></i>
            </p>
            <p class="control has-icon">
              <input class="input" name="password" placeholder="password" type="password">
              <i class="fa fa-lock"></i>
            </p>
            <div class="level is-mobile top-align">
              <div class="level-item" style="padding-right:5px">
                <span class="help is-marginless" :class="[error ? 'is-danger' : 'is-success']">{{ response }}</span>
              </div>
              <div class="level-item is-narrow">
                <button class="button submit is-success" @click.prevent="submit">Sign Up</button>
              </div>
            </div>
          </div>
        </div>
        <div class="column"></div>
      </div>
      <input type="hidden" name="contriGL" value="0">
      <input type="hidden" name="contriRL" value="0">
    </form>
  </section>
</template>

<style>
.signup .level-item { margin-top: 10px; }
.signup .level.top-align { align-items: flex-start; }
</style>

<script>
var serialize = require('form-serialize')
var request = require('superagent')
export default {
  name: 'UserProfileView',
  methods: {
    submit: function () {
      this.response = ''
      request.post(process.env.API_URL+'/user/')
      .send(serialize(this.$els.form, {hash: true}))
      .end((err, res) => {
        this.error = !!err || !res.ok
        this.response = this.error ? res.body.message : (res.text === '' ? 'success' : res.text)
      })
    }
  },
  data () {
    return {
      error: false,
      response: ''
    }
  }
}
</script>
