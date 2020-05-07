<template>
  <div>
    <p class="error">{{ error }}</p>

    <p class="decode-result">Last result: <b>{{ result }}</b></p>
    <p>  <strong>INFO: </strong> {{ info }} </p>

    <qrcode-stream @decode="onDecode" @init="onInit" />
  </div>
</template>

<script>
import { QrcodeStream } from 'vue-qrcode-reader'
import axios from "axios";

export default {

  components: { QrcodeStream },

  data () {
    return {
      result: '',
      rut: '',
      info: '',
      error: ''
    }
  },

  methods: {
    onDecode (result) {
      this.result = result
      this.getRut()
      this.getInfo()
    },

    getRut () {
      var regex = '[{0-9}]{7,}/?-.'
      var rut = this.result.match(regex)
      this.rut = rut
      alert(rut)

    },

    getInfo () {
      axios
            .get('https://siichile.herokuapp.com/consulta',{
            params: {
             'rut': this.rut[0]
            }
            })
            .then(response => {
              this.info = response.data
            })
            .catch(error => {
              console.log(error)
              this.errored = true
            })
        
    },

    async onInit (promise) {
      try {
        await promise
      } catch (error) {
        if (error.name === 'NotAllowedError') {
          this.error = "ERROR: you need to grant camera access permisson"
        } else if (error.name === 'NotFoundError') {
          this.error = "ERROR: no camera on this device"
        } else if (error.name === 'NotSupportedError') {
          this.error = "ERROR: secure context required (HTTPS, localhost)"
        } else if (error.name === 'NotReadableError') {
          this.error = "ERROR: is the camera already in use?"
        } else if (error.name === 'OverconstrainedError') {
          this.error = "ERROR: installed cameras are not suitable"
        } else if (error.name === 'StreamApiNotSupportedError') {
          this.error = "ERROR: Stream API is not supported in this browser"
        }
      }
    }
  }
}
</script>

<style scoped>
.error {
  font-weight: bold;
  color: red;
}
</style>
