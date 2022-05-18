<template>
<div>
<DropDown />
</div>
</template>

<script>
import DropDown from './components/Drop-down.vue'
export default {
  name: 'App',
  components: {
     DropDown
  },
}
</script>

<style>
#app {
   font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
  }
  *{
     padding: 0;
     margin: 0;
     box-sizing: border-box;
  }
</style>
