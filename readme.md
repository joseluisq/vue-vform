# vue-vform

> [Vue.js 2](https://vuejs.org/) form component that integrates [jQuery Validation](https://github.com/jquery-validation/jquery-validation) and [Axios](https://github.com/mzabriskie/axios).


## Install

```sh
npm install vue-vform --save-dev
```

## Prerequisites

- [Vue.js 2](https://vuejs.org/)
- [jQuery](https://github.com/jquery/jquery)
- [jQuery Validation](https://github.com/jquery-validation/jquery-validation)
- [Axios](https://github.com/mzabriskie/axios) (optional if you want to send an Ajax request after validation)

## Usage

Define `vform` component markup inside your custom component.

For example in your `custom-form-component.vue`:

```html
<template>
  <vform
    request
    :params="user"
    method="post"
    action="/api/v1/user/add"
    @validate="mySubmitCallback">

    <!-- Your cool stuff -->
    <div class="form-group">
      <label for="txtname">Name</label>
      <input
        name="txtname"
        v-model="user.name"
        required
        data-msg-required="Enter your name"
        type="text" class="form-control">
    </div>

    <div class="form-group">
      <label for="txtemail">E-mail</label>
      <input
        name="txtemail"
        v-model="user.email"
        required
        data-msg-required="Enter your E-mail"
        data-rule-email="true"
        data-msg-email="Enter a valid E-mail address"
        type="text" class="form-control">
    </div>
    <!-- //Your cool stuff -->

	</vform>
</template>

<script>
  export default {
    data() {
      return {
      	user: {
          name: '',
          email: ''
        }
      }
    }

    methods: {
      /**
       * Callback method when validation is completed.
       */
      mySubmitCallback (promise) {
        promise
          .then(response => response.data)
          .then(data => console.log(data))
          .catch(err => console.log(err.message))
      }
    }
  }
</script>
```

In your entry app:

```js
const Vue = require('vue')

// jQuery and jQuery Validation
window.$ = window.jQuery = require('jquery')
require('jquery-validation')

// If you want auto form Ajax request (optional)
window.axios = require('axios')

Vue.component('vform', require('vue-vform'))
Vue.component('custom-form-component', require('./components/custom-form-component'))

const app = new Vue({
  el: '#app'
})

```

## Attributes

#### method (optional)
The request method (POST, PUT, DELETE, etc). For dynamic value use `v-bind:method="myMethod"` or `:method="myMethod"`.

#### action (optional)
The request URL.

#### request (optional)

If `request` (Boolean) attribute is defined `vform` performs an Ajax Request using __Axios__ and a __Promise object__ is passed to your callback. Make sure that you have [Axios](https://github.com/mzabriskie/axios) before.

#### params (optional)

The component data binding (usually `FormData` or plain object `{}` values) that it will send if `request` option was setted. (use `v-bind:param="mydata"` or `:param="mydata"` property)

#### accept (optional)

The request `Accept` header. Default: `application/json`

## Events

#### @validate

Event when validation is completed. You need to pass the callback defined in your `methods: ...`. A Promise object will be passed if `request` attribute was defined.

## Tip
__Laravel v5.4 users__: It's necessary to define the [Axios](https://github.com/mzabriskie/axios) common headers in your `app.js` file. That's is useful when your use [Laravel v5.4](https://laravel.com/docs/5.4/) and [Passport](https://laravel.com/docs/5.4/passport).

```js
axios.defaults.headers.common = {
  'Accept': 'application/json',
  'X-Requested-With': 'XMLHttpRequest',
  'X-CSRF-TOKEN': Laravel.csrfToken
}
```

## License
MIT license

© 2017 [José Luis Quintana](http://git.io/joseluisq)
