<template>
    <section class="order-calc">

        <div class="order-calc-derails">
            <div class="calc-section">
                <div class="calc-left">
                    ${{ getCurrStayPrice }} x {{ getCurrNights }} nights
                </div>
                <div class="calc-right">
                    ${{ Math.round(getCurrNights * getCurrStayPrice).toLocaleString() }}
                </div>
            </div>
            <div class="calc-section">
                <div class="calc-left">
                    Service fee
                </div>
                <div class="calc-right">
                    ${{ Math.round((getCurrNights * getCurrStayPrice) / 9).toLocaleString() }}
                </div>
            </div>
        </div>
        <div class="final-calc">
            <div class="total-section">
                <div class="total-left">
                    Total
                </div>
                <div class="total-right">
                    ${{ Math.round(((getCurrNights * getCurrStayPrice) / 9 + getCurrNights * getCurrStayPrice)).toLocaleString()}}
                </div>
            </div>
        </div>

    </section>
</template>

<script>
export default {
    name: 'order-calc',
    computed: {
        getCurrStayPrice() {
            const currStay = this.$store.getters.getCurrStay
            return currStay.price
        },
        getCurrNights() {
            const chckIn = this.$store.getters.getCurrChckInDate
            const chckOut = this.$store.getters.getCurrChckOutDate
            const chckInMls = chckIn.getTime()
            const chckOutMls = chckOut.getTime()
            const diff = chckOutMls - chckInMls
            const diffInDays = diff / (1000 * 3600 * 24)
            return diffInDays
        },
        getCurrTotal() {
            const currNights = this.getCurrNights
            const currStayPrice = this.getCurrStayPrice
            return currNights * currStayPrice
        }
    }

}
</script>

<style lang="sass">

</style>