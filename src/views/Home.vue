<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>Zip Info</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <ZipSearch @get-zip="getZipInfo"/>
      <ZipInfo :info="info"/>
      <ClearInfo :info="info" @clear-info="clearInfo"/>

    </ion-content>

  </div>
</template>

<script>
// @ is an alias to /src
import ZipSearch from "../components/ZipSearch"
import ZipInfo from "../components/ZipInfo"
import ClearInfo from "../components/ClearInfo"


export default {
  name: 'home',
  data() {
    return {
      info: null
    }
  },
  components: {
    ZipSearch,
    ZipInfo,
    ClearInfo

  },
  methods: {
    async getZipInfo(zip) {
      const res = await fetch(`https://api.zippopotam.us/us/${zip}`)
      if (res.status == 404) {
        this.showAlert()
      }
      const info = await res.json()
      this.info = info
      //console.log(info)
    },
    clearInfo() {
      this.info = null
    },
    showAlert() {
      return this.$ionic.alertController
        .create({
          header: "Not Valid",
          message: "Please enter a valid US zipcode",
          buttons: ["Ok"]
        })
        .then(a => a.present())
    }
  }
}
</script>
