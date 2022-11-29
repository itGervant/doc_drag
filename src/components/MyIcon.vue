<template>
  <svg 
   focusable="false" width="24" height="24" 
  viewBox="0 0 128 128" xmlns="http://www.w3.org/2000/svg"></svg>
</template>

<script>
import icons from "@/icons"

export default {
props: ['name'],

data() {
    return {
        path: icons[this.name]
    }
}
}
</script>

<style>

</style>