<template>
    <div id="plan-create">
      <div class="vx-row"> 
        <div class="vx-col w-full md:w-1/4">
          <vs-list>
            <vs-list-header title="Team 1" color="primary"></vs-list-header>
              <draggable :list="list1" group="people" class="p-2 cursor-move">
                <vs-list-item 
                  v-for="(listItem, index) in list1" 
                  :key="index" 
                  :title="listItem.name" 
                  :subtitle="listItem.email">
                  <vs-avatar slot="avatar" :text="listItem.name" />
                </vs-list-item>
              </draggable>
          </vs-list>
        </div>
        <div class="vx-col w-full md:w-1/4">
          <vs-list>
              <vs-list-header title="Team 2" color="primary"></vs-list-header>
              <draggable :list="list2" group="people" class="p-2 cursor-move">
                <vs-list-item 
                  v-for="(listItem, index) in list2" 
                  :key="index" 
                  :title="listItem.name" 
                  :subtitle="listItem.email">
                  <vs-avatar slot="avatar" :text="listItem.name" />
                </vs-list-item>
              </draggable>
          </vs-list>
        </div>
      </div>
    </div>
  </template>
  
  
<script>
import VueApexCharts from 'vue-apexcharts'
import StatisticsCardLine from '@/components/statistics-cards/StatisticsCardLine.vue'
import analyticsData from '../ui-elements/card/analyticsData.js'
import ChangeTimeDurationDropdown from '@/components/ChangeTimeDurationDropdown.vue'
import VxTimeline from '@/components/timeline/VxTimeline'
import apexChatData from '../charts-and-maps/charts/apex-charts/apexChartData.js'
import draggable from 'vuedraggable'

export default {
  data () {
    return {

      apexChatData,
      checkpointReward: {},
      subscribersGained: {},
      ordersRecevied: {},
      salesBarSession: {},
      supportTracker: {},
      productsOrder: {},
      salesRadar: {},
      list1: [
        {
          name: 'Paz Joya',
          email: 'girliness@spotlike.co.uk'
        },
        {
          name: 'Sunshine Cose',
          email: 'executrixship@equisized.edu'
        },
        {
          name: 'Alba Dobbin',
          email: 'bidding@demibob.or'
        },
        {
          name: 'Marlin Hinchee',
          email: 'preholding@scuffly.co.uk'
        }
      ],
      list2: [
        {
          name: 'Leia Atienza',
          email: 'unmeasurableness@interlamellar.co.uk'
        },
        {
          name: 'Lashawna Vaudrainm',
          email: 'soaking@khubber.com'
        },
        {
          name: 'Liliana Henscheid',
          email: 'lecideine@turndown.org'
        },
        {
          name: 'Keven Kolter',
          email: 'nontenure@anglicanum.co.uk'
        }
      ],
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
      pieChart: {
        series: [44, 55, 13, 43],
        chartOptions: {
          labels: ['Team A', 'Team B', 'Team C', 'Team D'],
          colors: ['#7367F0', '#28C76F', '#EA5455', '#FF9F43', '#1E1E1E'],
          responsive: {
            breakpoint: 480,
            options: {
              chart: {
                width: 200
              },
              legend: {
                position: 'bottom'
              }
            }
          }
        }
      },

      analyticsData,
      dispatchedOrders: []
    }
  },
  components: {
    VueApexCharts,
    StatisticsCardLine,
    ChangeTimeDurationDropdown,
    VxTimeline,
    draggable
  },
  beforeCreate () {
    if (!this.$store.state.auth.isUserLoggedIn()) this.$router.push('/login')
    //  User Reward Card
  },
  created () {
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
  