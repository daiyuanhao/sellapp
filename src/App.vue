<template>
	<div id="app">
		<v-header :seller="seller"></v-header>
		<tab></tab>
		<keep-alive>
			<router-view :seller="seller"></router-view>
		</keep-alive>
	</div>
</template>

<script>
import {urlParse} from './common/js/util.js'
import header from './components/header/header.vue'
import tab from './components/tab.vue'

export default {
  name: 'App',
  data () {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          return queryParam.id
        })()
      }
    }
  },
  created () {
    this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      response = response.body
      if (response.errno === 0) {
        this.seller = Object.assign({}, this.seller, response.data)
      }
    })
  },
  components: {
    'v-header': header,
    tab
  }
}
</script>

<style>
	@import url("reset.css");

</style>
