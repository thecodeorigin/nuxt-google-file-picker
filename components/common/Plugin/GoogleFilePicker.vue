<template>
  <div class="text-center google-file-picker-container">
    <b-button
      variant="success"
      class="mt-3 google-file-picker"
      @click="pickFile()"
    >
      Pick an image
    </b-button>
    <div />
    <b-img :src="imageUrl" thumbnail rounded class="mt-3" />
  </div>
</template>

<script>
// You must disable eslint or it will throw "google is not defined"
/*eslint-disable*/
export default {
  data() {
    return {
      pickerApiLoaded: false,
      pickedImageId: ''
    }
  },
  computed: {
    imageUrl() {
      return 'https://drive.google.com/uc?id=' + this.pickedImageId
    }
  },
  mounted() {
    // window only usable in mounted hook
    if (!window.gapi) {
      return
    } else {
      window.gapi.load('auth2', () => {
        window.gapi.load('picker', () => {
          this.pickerApiLoaded = true
        })
      })
    }
  },
  methods: {
    pickFile() {
      if (!this.$store.state.googleOAuthToken) {
        alert('You must signin to use this feature')
      } else {
        this.createPicker()
      }
      // this.createPicker()
    },
    createPicker() {
      if (this.pickerApiLoaded && this.$store.state.googleOAuthToken) {
        var picker = new google.picker.PickerBuilder()
          //   .enableFeature(google.picker.Feature.NAV_HIDDEN)
          //   .enableFeature(google.picker.Feature.MULTISELECT_ENABLED)
          .setAppId(process.env.PROJECT_ID)
          .setOAuthToken(this.$store.state.googleOAuthToken)
          .addView(
            new google.picker.DocsView()
              .setIncludeFolders(true)
              .setOwnedByMe(true)
          ) // My Drive
          .addView(new google.picker.DocsUploadView().setIncludeFolders(true)) // Upload view
          .addView(
            new google.picker.DocsView(google.picker.ViewId.RECENTLY_PICKED)
          ) // Recently picked
          .addView(
            new google.picker.DocsView()
              .setIncludeFolders(true)
              .setOwnedByMe(false)
          ) // Shared with me
          .addView(
            new google.picker.DocsView()
              .setIncludeFolders(true)
              .setStarred(true)
              .setLabel('Starred')
          ) // Starred
          .setLocale('en')
          .setDeveloperKey(process.env.API_KEY)
          .setCallback(this.pickerCallback)
          .build()
        picker.setVisible(true)
      }
    },
    pickerCallback(data) {
      if (data[google.picker.Response.ACTION] == google.picker.Action.PICKED) {
        this.pickedImageId = data.docs[0].id
      }
    }
  }
}
</script>

<style scoped>
.google-file-picker-container {
  margin: auto;
  width: 320px;
}
.google-file-picker {
  border-radius: 0 !important;
}
</style>
