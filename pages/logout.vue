<template>
    <div></div>
</template>
<script>
export default {
  mounted() {
    sessionStorage.removeItem('username')
    sessionStorage.removeItem('user_id')
    sessionStorage.removeItem('password')
    sessionStorage.removeItem('status')
    sessionStorage.removeItem('user_level')
    this.$router.push({path: '/login' })
  }
};
</script>
