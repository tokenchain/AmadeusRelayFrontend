<template>
  <div class="form-group">
    <select v-model="tokenSelected" class="form-control">
      <option value="">All tokens</option>
      <option v-for="(coin, index) in tokenList" :value="coin" :key="coin">{{ coin }}</option>
    </select>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator'
import { Getter } from 'vuex-class'

@Component
export default class TokensList extends Vue {
  tokenList: string[] = []
  tokenSelected = ''

  @Getter getTokenPairs

  mounted () {
    this.refreshToken('', null)
    this.tokenSelected = this.token;
  }

  refreshToken (tokenSelected, takerIsSelected) {
    var tokens = this.getTokenPairs
    if (tokenSelected) {
      if (takerIsSelected) {
        this.tokenList = tokens.filter(a => a.tokenASymbol === tokenSelected).map(i => i.tokenBSymbol)
      } else {
        this.tokenList = tokens.filter(a => a.tokenBSymbol === tokenSelected).map(i => i.tokenASymbol)
      }
    } else {
      this.tokenList = tokens.map(i => i.tokenASymbol).filter((item, pos, arr) => arr.indexOf(item) === pos)
    }
  }

  @Prop()
  token: string
  @Prop()

  @Watch('tokenSelected')
  onPropertyChanged (value: string, oldValue: string) {
    this.$emit('updateToken', value)
  }
}
</script>

<style scoped>

select.form-control {
  cursor: pointer;
  border-radius: 3px;
  background-color: rgba(119, 117, 144, 0.5);
  color: #ffffff;
  border: none;
  height: 52px !important;
}

</style>