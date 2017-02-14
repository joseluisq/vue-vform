<template>
  <form :id="id" :request="request" :class="classes" :name="name"
    :action="action" :method="method" :accept="accept">
    <slot></slot>
  </form>
</template>

<script>
  export default {
    props: {
      'id': String,
      'name': String,
      'classes': String,
      'request': {
        type: Boolean,
        default: false
      },
      'params': {
        type: Object,
        default: {}
      },
      'action': {
        type: String,
        required: false
      },
      'method': {
        type: String,
        required: true,
        default: 'POST',
        validator: value => {
          switch (value.toUpperCase()) {
            case 'POST': return true
            case 'PUT': return true
            case 'DELETE': return true
            default: return false
          }
        }
      },
      'accept': {
        type: String,
        default: 'application/json'
      }
    },

    mounted () {
      this.prepareComponent()
    },

    methods: {
      prepareComponent () {
        $(this.$el).validate({
          errorPlacement: (error, element) => {
            $(element).parent().append(error)
          },
          highlight: (element, errorClass) => {
            $(element).addClass(errorClass)
          },
          unhighlight: (element, errorClass) => {
            $(element).removeClass(errorClass)
          },
          submitHandler: () => {
            let promise = null

            if (this.request) {
              promise = axios({
                url: this.action,
                headers: {
                  'Accept': this.accept,
                  'X-Requested-With': 'XMLHttpRequest'
                },
                method: this.method,
                data: this.params
              })
            }

            this.$emit('validate', promise)
          }
        })
      }
    }
  }
</script>
