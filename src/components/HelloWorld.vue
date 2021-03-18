<template>
  <div class="hello">
      {{msg}}
      <br/>
      <br/>
      <div class="row">
    <button @click="useAuthProvider('github', null)">oAuth Github M1</button>
<!-- <button @click="useAuthProvider('facebook', null)">auth Facebook M1</button>
<button @click="useAuthProvider('google', null)">auth Google M1</button>
<button @click="useAuthProvider('twitter', null)">auth Twitter M1</button> -->

</div>
<br/>
      <br/>
      <div class="row">
    <button @click="useAuthProvider('github', Github)">oAuth Github M2</button>
<!-- <button @click="useAuthProvider('facebook', Facebook)">auth Facebook M2</button>
<button @click="useAuthProvider('google', Google)">auth Google M2</button>
<button @click="useAuthProvider('twitter', Twitter)">auth Twitter M2</button>
<button @click="useAuthProvider('mycustom', Mycustom)">auth Mycustom M2</button> -->

</div>
  </div>
</template>

<script>

// Button M1
import { Providers} from 'universal-social-auth'

// Button M2
import { Github /*Facebook, Google , Twitter*/} from 'universal-social-auth'

export default {
  name: 'HelloWorld',
 props: {
    msg: String
  },
  data() {
    return {
      responseData: {},
      hash: '',
      data: {},
      fauth: false,
      Github,
    //   Facebook,
    //   Google,
    //   Twitter
    }
  },

  methods: {
    useAuthProvider (provider, proData) {
      const pro = proData

      const ProData = pro || Providers[provider]
      this.$Oauth.authenticate(provider, ProData).then((response) => {
        const rsp = response
        if (rsp.code) {
          this.responseData.code = rsp.code
          this.responseData.provider = provider
          this.useSocialLogin()
        }
      }).catch((err) => {
        console.log(err)
      })
    },
    useSocialLogin () {
      // otp from input Otp form
      // hash user data in your backend with Cache or save to database
      const pdata = { code: this.responseData.code, otp: this.data.tok, hash: this.hash }
      this.$axios.post('/social-login/' + this.responseData.provider, pdata).then(async (response) => {
          // `response` data base on your backend config
        if (response.data.status === 444) {
          this.hash = response.data.hash
          this.fauth = true // Option show Otp form incase you using 2fa or any addition security apply to your app you can handle all that from here

        }else if (response.data.status === 445) {
          //do something Optional

        }else {

          await this.useLoginFirst(response.data.u)
        }
      }).catch((err) => {

        console.log(err)
      })
    },
  async  useLoginFirst (e) {
    // this sample of to pust user data to my store
    console.log('userdata', e)
    }
  }
}
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
