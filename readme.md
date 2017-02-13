# vue-vform

> [Vue.js 2](https://vuejs.org/) form component that integrate [jQuery Validation](https://github.com/jquery-validation/jquery-validation) and [Axios](https://github.com/mzabriskie/axios).


## Install

```sh
npm install vue-vform --save-dev
```

## Prerequisites

- [Vue.js 2](https://vuejs.org/)
- [jQuery Validation](https://github.com/jquery-validation/jquery-validation)
- [Axios](https://github.com/mzabriskie/axios) (optional)

## Usage

Define `vform` component markup inside your custom component.

For example in your `custom-form-component.vue`:

```html
<template>
  <vform
    request
    accept="application/json"
    params="user"

    id="myform"
    method="post"
    action="/api/v1/user/add"
    @validate="mySubmitCallback">

    <!-- Bootstrap markup example -->
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

Vue.component('vform', require('vue-vform'))
Vue.component('custom-form-component', require('./components/custom-form-component'))

const app = new Vue({
  el: '#app'
})

```

## Attributes

#### method
The request method (POST, PUT, DELETE, etc)

#### action
The request URL.

#### params

The  Vue.js `data` key name that it will send.

#### request (optional)

If `request` attribute is defined in __vform__ markup an __Axios Promise object__ is passed to your callback.

#### accept (optional)

The request `Accept` header. Default: "application/json"

## Events

#### @validate

Event when validation is completed.

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
