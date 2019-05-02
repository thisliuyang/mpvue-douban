<template>
  <div>item</div>
</template>
<script>
import { findOne } from '../../utils/api'
export default {
  data () {
    return {
      title: '',
      movie: null
    }
  },
  onShow () {
    let id = this.$root.$mp.query.id
    findOne(id).then(res => {
      this.title = res.title
      this.movie = res
    })
  }
}
</script>
<style scoped>
</style>