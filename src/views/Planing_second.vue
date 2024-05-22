<template>
  <div id="dashboard-analytics">

    <div id="row_first" class="vx-row">
      <div id="row_1_temp-col" class="vx-col w-full md:w-1/4 mb-base"></div>
      <div id="row_1_main_col" class="vx-col w-full md:w-1/2 mb-base">
        <vx-card class="rounded-10" style="border : 3px solid black; border-radius: 200px; background-color: #f05a5b; " >
          <div class="vx-row flex-col-reverse md:flex-col-reverse sm:flex-row lg:flex-row"
          >
            <!-- LEFT COL -->
            <div id="circle"
              class="vx-col w-full md:w-full sm:w-1/4 lg:w-1/4 xl:w-1/4 flex flex-col justify-between bg-cyan ms-5 shadow" style="border : 3px solid white; border-radius: 200px; background-color: #fdfdfd; margin-left: 10px; box-shadow: 0 2px 4px 0 black !important">
            </div>
            <div id="100k"
              class="vx-col w-full md:w-full sm:w-1/3 lg:w-1/3 xl:w-1/3 flex flex-col justify-between bg-cyan"
              v-if="salesBarSession.analyticsData"
            >
              <div id="+5.2%">
                <h2 class="mb-1 font-bold">
                  {{ salesBarSession.analyticsData.session | k_formatter }}
                </h2>
                <!-- <span class="font-medium">Avg Sessions</span> -->

                <!-- Previous Data Comparison -->
                <p class="mt-2 text-xl font-medium">
                  <span
                    :class="
                      salesBarSession.analyticsData.comparison.result >= 0
                        ? 'text-success'
                        : 'text-danger'
                    "
                  >
                    <span style="font-size: 12px;" v-if="salesBarSession.analyticsData.comparison.result > 0"
                      >+</span
                    >
                    <span style="font-size: 12px;">{{ salesBarSession.analyticsData.comparison.result }}</span>
                  </span>
                  <span style="font-size: 12px;"> % vs </span>
                  <span style="font-size: 12px;">{{ salesBarSession.analyticsData.comparison.str }}</span>
                </p>
              </div>
              <vs-button
                icon-pack="feather"
                icon="icon-chevrons-right"
                icon-after
                class="shadow-md w-full lg:mt-0 mt-4 p-2"
                >View Details</vs-button
              >
            </div>
            <!-- RIGHT COL -->
            <div
              class="vx-col w-full md:w-full sm:w-1/3 lg:w-1/3 xl:w-1/3 flex flex-col lg:mb-0 md:mb-base sm:mb-0 mb-base"
            >
              <!-- <change-time-duration-dropdown class="self-end" /> -->
              <vue-apex-charts
                type="bar"
                height="140"
                :options="analyticsData.salesBar.chartOptions"
                :series="salesBarSession.series"
                v-if="salesBarSession.series"
              />
            </div>
          </div>
        </vx-card>
      </div>
    </div>

    <div id="row_second" class="vx-row">
      <div id="row_1_temp-col" class="vx-col w-full md:w-1/3 mb-base"></div>
      <div id="row_1_main_col" class="vx-col w-full md:w-1/2 mb-base">
        <vx-card class="rounded-10" style="border : 3px solid red; border-radius: 200px;">
          <div class="vx-row flex-col-reverse md:flex-col-reverse sm:flex-row lg:flex-row"
          >
            <!-- LEFT COL -->
            <div id="circle"
              class="vx-col w-full md:w-full sm:w-1/4 lg:w-1/4 xl:w-1/4 flex flex-col justify-between bg-cyan " style="border : 3px solid red; border-radius: 200px; background-color: gold;"
            >
            </div>
            <div id="100k"
              class="vx-col w-full md:w-full sm:w-1/3 lg:w-1/3 xl:w-1/3 flex flex-col justify-between bg-cyan"
              v-if="salesBarSession.analyticsData"
            >
              <div id="+5.2%">
                <h2 class="mb-1 font-bold">
                  {{ salesBarSession.analyticsData.session | k_formatter }}
                </h2>
                <p class="mt-2 text-xl font-medium">
                  <span
                    :class="
                      salesBarSession.analyticsData.comparison.result >= 0
                        ? 'text-success'
                        : 'text-danger'
                    "
                  >
                    <span style="font-size: 12px;" v-if="salesBarSession.analyticsData.comparison.result > 0"
                      >+</span
                    >
                    <span style="font-size: 12px;">{{ salesBarSession.analyticsData.comparison.result }}</span>
                  </span>
                  <span style="font-size: 12px;"> % vs </span>
                  <span style="font-size: 12px;">{{ salesBarSession.analyticsData.comparison.str }}</span>
                </p>
              </div>
              <vs-button
                icon-pack="feather"
                icon="icon-chevrons-right"
                icon-after
                class="shadow-md w-full lg:mt-0 mt-4 p-2"
                >View Details</vs-button
              >
            </div>
            <!-- RIGHT COL -->
            <div
              class="vx-col w-full md:w-full sm:w-1/3 lg:w-1/3 xl:w-1/3 flex flex-col lg:mb-0 md:mb-base sm:mb-0 mb-base"
            >
              <vue-apex-charts
                type="bar"
                height="140"
                :options="analyticsData.salesBar.chartOptions"
                :series="salesBarSession.series"
                v-if="salesBarSession.series"
              />
            </div>
          </div>
        </vx-card>
      </div>
    </div>
  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'
import StatisticsCardLine from '@/components/statistics-cards/StatisticsCardLine.vue'
import analyticsData from './ui-elements/card/analyticsData.js'
import ChangeTimeDurationDropdown from '@/components/ChangeTimeDurationDropdown.vue'
import VxTimeline from '@/components/timeline/VxTimeline'

export default {
  data () {
    return {
      checkpointReward: {},
      subscribersGained: {},
      ordersRecevied: {},
      salesBarSession: {},
      supportTracker: {},
      productsOrder: {},
      salesRadar: {},

      timelineData: [
        {
          color: 'primary',
          icon: 'PlusIcon',
          title: 'Client Meeting',
          desc: 'Bonbon macaroon jelly beans gummi bears jelly lollipop apple',
          time: '25 mins Ago'
        },
        {
          color: 'warning',
          icon: 'MailIcon',
          title: 'Email Newsletter',
          desc: 'Cupcake gummi bears soufflé caramels candy',
          time: '15 Days Ago'
        },
        {
          color: 'danger',
          icon: 'UsersIcon',
          title: 'Plan Webinar',
          desc: 'Candy ice cream cake. Halvah gummi bears',
          time: '20 days ago'
        },
        {
          color: 'success',
          icon: 'LayoutIcon',
          title: 'Launch Website',
          desc:
            'Candy ice cream cake. Halvah gummi bears Cupcake gummi bears soufflé caramels candy.',
          time: '25 days ago'
        },
        {
          color: 'primary',
          icon: 'TvIcon',
          title: 'Marketing',
          desc: 'Candy ice cream cake. Halvah gummi bears Cupcake gummi bears.',
          time: '28 days ago'
        }
      ],

      analyticsData,
      dispatchedOrders: []
    }
  },
  components: {
    VueApexCharts,
    StatisticsCardLine,
    ChangeTimeDurationDropdown,
    VxTimeline
  },
  beforeCreate() {
    if (!this.$store.state.auth.isUserLoggedIn()) this.$router.push('/login')
    //  User Reward Card
  },
  created() {
    // if (!this.$store.state.auth.isUserLoggedIn()) this.$router.push({ path: '/login' })
    //  User Reward Card
    this.$http
      .get('/api/user/checkpoint-reward')
      .then((response) => {
        this.checkpointReward = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Subscribers gained - Statistics
    this.$http
      .get('/api/card/card-statistics/subscribers')
      .then((response) => {
        this.subscribersGained = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Orders - Statistics
    this.$http
      .get('/api/card/card-statistics/orders')
      .then((response) => {
        this.ordersRecevied = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Sales bar - Analytics
    this.$http
      .get('/api/card/card-analytics/sales/bar')
      .then((response) => {
        this.salesBarSession = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Support Tracker
    this.$http
      .get('/api/card/card-analytics/support-tracker')
      .then((response) => {
        this.supportTracker = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Products Order
    this.$http
      .get('/api/card/card-analytics/products-orders')
      .then((response) => {
        this.productsOrder = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Sales Radar
    this.$http
      .get('/api/card/card-analytics/sales/radar')
      .then((response) => {
        this.salesRadar = response.data
      })
      .catch((error) => {
        console.log(error)
      })

    // Dispatched Orders
    this.$http
      .get('/api/table/dispatched-orders')
      .then((response) => {
        this.dispatchedOrders = response.data
      })
      .catch((error) => {
        console.log(error)
      })
  }
}
</script>

<style lang="scss">
/*! rtl:begin:ignore */
#dashboard-analytics {
  .greet-user {
    position: relative;

    .decore-left {
      position: absolute;
      left: 0;
      top: 0;
    }
    .decore-right {
      position: absolute;
      right: 0;
      top: 0;
    }
  }

  @media (max-width: 576px) {
    .decore-left,
    .decore-right {
      width: 140px;
    }
  }
}
/*! rtl:end:ignore */
</style>
