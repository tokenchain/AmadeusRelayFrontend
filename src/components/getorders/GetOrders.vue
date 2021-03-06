<template>
<div id="get-orders-section">
      <div class="container">
          <div class="row">
            <div class="col-md-12">
                <p>Amadeus returns an array of tokens, combined in pairs. Then the dApp choose which tokens to trade using endpoint <b>GET orders</b>.</p>
            </div>
          </div>
          <div class="row">
            <div class="col-md-4">
              <label>Taker token (to sell)</label>
              <tokens-list :token='takerToken' ref='takerTokenRef' @updateToken='updateTakerToken'/> 
            </div>
            <div class="col-md-4">
              <label>Maker token (to buy)</label>
              <tokens-list :token='makerToken' ref="makerTokenRef" @updateToken='updateMakerToken'/>
            </div>
          </div> 
          <div class="row">
            <div class="col-md-12">
                <a class="btn-next-step" @click="goToChooseOrdersPage()">GET ORDERS
                <img src="../../assets/arrow-right.svg"/>
                </a>
            </div>
        </div>
      </div>
</div>
</template>


<script lang="ts">
import TokensList from './TokensList.vue'
import { Component, Vue } from 'vue-property-decorator'
import { Mutation } from 'vuex-class'
import { OrderService, ZeroXService } from '../../api'
import { Scripts } from '../../utils/scripts'

@Component({
  components: { 'tokens-list': TokensList }
})
export default class GetOrders extends Vue {
  takerToken: string = ''
  makerToken: string = ''

  $refs: {
    makerTokenRef: TokensList,
    takerTokenRef: TokensList
  }

  @Mutation addCodeLine
  @Mutation changePage
  @Mutation updateOrders
  @Mutation updateLoadingState

  goToChooseOrdersPage () {
    var orderService : OrderService = new OrderService(new ZeroXService());
    this.updateLoadingState(true)
    orderService.listOrders(this.takerToken, this.makerToken).then(this.onSuccessfullyListOrders);
  }

  onSuccessfullyListOrders (orders: any) {
    this.updateOrders(orders);
    this.updateLoadingState(false)
    this.changePage(3);
  }

  updateTakerToken (value : string) {
    this.takerToken = value
    this.$refs.makerTokenRef.refreshToken(this.takerToken, true)
  }

  updateMakerToken (value : string) {
    this.makerToken = value
    this.$refs.takerTokenRef.refreshToken(this.makerToken, false)
  }

  mounted () {
    this.addCodeLine(new Scripts().getOrders);
  }
}
</script>

<style scoped>
#get-orders-section .container {
    padding-top: 130px;
}

#get-orders-section .container p{
    font-size: 22px;
    color: #ffffff;
    font-weight: 300;
}

#get-orders-section .container p b{
    font-weight: 400;
}

#get-orders-section label{
    color: #ffffff;
    font-size: 17px;
}

@media (max-width: 992px){
  #get-orders-section .container p{
    font-size: 20px;
  }
}
</style>