<template>
    <section class="reserve-table">
        <div class="order-data">
            <label class="date-picker">
                <div class="date-input">
                    <div class="date-head-reserve-table">check-in</div>
                    <div class="date-reserve-table">{{ startDate }}</div>
                </div>
                <div class="date-input last">
                    <div class="date-head-reserve-table">checkout</div>
                    <div class="date-reserve-table">{{ endDate }}</div>
                </div>
                <el-date-picker class="date-picker-container" v-model="tripDates" type="daterange"
                    @change="setTripDates" range-separator="To" start-placeholder="Start date"
                    end-placeholder="End date" />
            </label>
            <div class="guest-input">
                <div class="adding-guests" @click="guestsModalShown = !guestsModalShown">
                    <div class="right-side">
                        <label for="guests-number">guests</label>
                        <div id="guests-number" class="guests-number">
                            {{ getTotalGuests }}
                        </div>
                    </div>
                    <div v-if="guestsModalShown" class="arrow-down">
                        <arrow-down-svg />
                    </div>
                    <div v-else class="arrow-down">
                        <arrow-up-svg />
                    </div>

                </div>


                <div class="guests-modal-reserve" v-if="guestsModalShown">
                    <div class="guests-type">
                        <div class="guests-type-view">
                            <div class="guests-type-view">
                                <div class="title">
                                    Adults
                                </div>
                                <div class="second-title">
                                    Ages 13 +
                                </div>
                            </div>
                        </div>
                        <div class="guests-type-selection">
                            <button :disabled="getAdultsNum <= 0" class="btn-round" @click="setAdults(-1)">-</button>
                            <span>{{ getAdultsNum }}</span>
                            <button :disabled="getAdultsNum >= 10" class="btn-round" @click="setAdults(1)">+</button>
                        </div>
                    </div>
                    <div class="guests-type">
                        <div class="guests-type-view">
                            <div class="title">
                                Children
                            </div>
                            <div class="second-title">
                                Ages 2-12
                            </div>
                        </div>
                        <div class="guests-type-selection">
                            <button :disabled="getChildrenNum <= 0" class="btn-round"
                                @click="setChildren(-1)">-</button>
                            <span>{{ getChildrenNum }}</span>
                            <button :disabled="getChildrenNum >= 10" class="btn-round"
                                @click="setChildren(1)">+</button>
                        </div>
                    </div>
                    <div class="close">
                        <div class="close-btn" @click="guestsModalShown = !guestsModalShown">Close</div>
                    </div>
                </div>


            </div>
        </div>
    </section>
</template>

<script>
import arrowDownSvg from './svg/arrow-down-svg.vue'
import arrowUpSvg from './svg/arrow-up-svg.vue'
export default {
    name: 'reserve-table',
    props: {
        stay: Object
    },
    data() {
        return {
            tripDates: null,
            guestsModalShown: false
        }
    },
    methods: {
        setTripDates() {
            const chckInDate = this.tripDates[0]
            const chckOutDate = this.tripDates[1]
            this.$store.commit({
                type: 'setTripDates',
                chckInDate, chckOutDate
            })
        },
        setAdults(diff) {
            const adultsNum = this.getAdultsNum
            const newAdultsNum = adultsNum + diff
            this.$store.commit({ type: 'setAdults', newAdultsNum })
        },
        setChildren(diff) {
            const childrenNum = this.getChildrenNum
            const newChildrenNum = childrenNum + diff
            this.$store.commit({ type: 'setChildren', newChildrenNum })
        }
    },
    computed: {
        startDate() {
            const chckInDate = this.$store.getters.getCurrChckInDate
            if (!chckInDate) return 'Add date'
            return chckInDate.toLocaleDateString()
        },
        endDate() {
            const ckckOutDate = this.$store.getters.getCurrChckOutDate
            if (!ckckOutDate) return 'Add date'
            return ckckOutDate.toLocaleDateString()
        },
        guests() {
            return this.$store.getters.getGuests
        },
        getAdultsNum() {
            return this.$store.getters.getAdultsNum
        },
        getChildrenNum() {
            return this.$store.getters.getChildrenNum
        },
        getTotalGuests() {
            const adults = this.$store.getters.getAdultsNum
            const children = this.$store.getters.getChildrenNum
            const sum = adults + children
            if (!sum) return 'Add guests'
            if (sum === 1) return '1 guest'
            else return `${sum} guests`
        }
    },
    created() {
        const chckInDate = this.$store.getters.getCurrChckInDate
        const chckOutDate = this.$store.getters.getCurrChckOutDate
        this.tripDates = [chckInDate, chckOutDate]
    },
    components: {
        arrowDownSvg,
        arrowUpSvg
    }

}

</script>

<style lang="scss">
.date-picker-container {
    z-index: 1 !important;
    opacity: 0 !important;
    width: 270px !important;
    cursor: pointer !important;
}

.el-date-table td.today .el-date-table-cell__text {
    color: black !important;
}
</style>