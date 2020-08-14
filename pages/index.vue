<template>
  <div class="ml-5">
    <h1 class="title">AmazonConnectStreamsのデモです！</h1>
    <p class="is-primary">{{ this.message }}</p>
    <div id=ccpContainer style="width: 320px; min-width: 200px; height: 465px; min-height: 400px; ">
    </div>
    <button class="mt-5 button is-rounded is-primary" @click="phoneCall">発信する</button>
  </div>
</template>

<script>
import 'amazon-connect-streams'

export default {
  data() {
    return {
      connect: null,
      currentAgent: null,
      message: ''
    }
  },
  mounted() {
    connect.core.initCCP(ccpContainer, {
      ccpUrl: process.env.NUXT_ENV_CCP_URL,
      loginPopup: true,
      softphone: {
          allowFramedSoftphone: true,
          disableRingtone: false,
      }
    })
    connect.agent(agent => {
      this.currentAgent = agent
      console.log(this.currentAgent)
    })
  },
  methods: {
    phoneCall() {
      var endpoint = connect.Endpoint.byPhoneNumber(process.env.NUXT_ENV_TARGET_PHONE_NUMBER);
      this.currentAgent.connect(endpoint,{
          success: function () {
              this.message = "Connect Success"
          },
          failure: function () {
              this.message = "Connect Failed"
          }
      });
    }
  }
}
</script>

<style>
.title {
    display: block;
    font-weight: 3;
    font-size: 30px;
    color: #35495e;
    letter-spacing: 1px;
}
</style>
