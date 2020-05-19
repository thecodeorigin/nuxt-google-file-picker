<template>
  <b-container fluid>
    <ExampleComponent />
    <GoogleSigninButton />
    <GoogleFilePicker />
  </b-container>
</template>

<script>
export default {
  name: 'Home',
  components: {
    ExampleComponent: () =>
      import('~/components/specifics/Home/ExampleComponent.vue'),
    GoogleSigninButton: () =>
      import('../components/common/Plugin/GoogleSigninButton'),
    GoogleFilePicker: () =>
      import('../components/common/Plugin/GoogleFilePicker')
  },
  created() {
    // Global function and variable from mixins
    // this.sayHello("Rim")
    // console.log(this.baseUrl)
  },
  head() {
    return {
      title: 'Home page',
      description: 'A short dummy description'
    }
  }
}
</script>
