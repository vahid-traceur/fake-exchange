<template>
  <v-card class="px-4" color="transparent" flat>
    <v-tabs v-model="tab" align-tabs="title" bg-color="transparent">
      <v-tab v-for="(item, i) in items" :key="i" :class="{'text-grey' : item !== tab}" :value="item" class="ma-0">
        {{ item }}
      </v-tab>
    </v-tabs>

    <v-divider/>

    <v-card-title class="d-flex align-center px-0">
      <v-btn class="px-1" prepend-icon="mdi-filter-outline" variant="text">
        Filter
      </v-btn>

      <v-divider class="align-self-center" style="height: 20px" vertical/>

      <v-btn class="mx-2" density="comfortable" icon variant="text">
        <v-icon>mdi-calendar-blank-outline</v-icon>
      </v-btn>

      <v-spacer/>

      <v-btn class="px-1" variant="text">
        <v-icon color="primary" start>mdi-check-circle</v-icon>
        Closed orders only
      </v-btn>
    </v-card-title>

    <v-divider/>

    <v-window v-model="tab" class="bg-transparent">
      <v-window-item value="Order History">
        <v-card-text>
          <v-row no-gutters>
            <v-col cols="12">
              <v-checkbox v-model="obj.short" label="short/long"/>
            </v-col>

            <v-col cols="12">
              <v-select v-model="obj.status" :items="['Isolated', 'Cross']" label="status"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.multiple" label="multiple"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.filled" label="filled"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.value" label="filled value"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.price" label="price"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.realized" label="realized"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.tradingFee" label="trading fee"/>
            </v-col>

            <v-col cols="12">
              <v-text-field v-model="obj.date" label="date"/>
            </v-col>
          </v-row>
        </v-card-text>

        <v-card-actions>
          <v-btn color="error" rounded="lg" variant="outlined" @click="obj = temp">
            reset
          </v-btn>

          <v-spacer/>

          <v-btn color="rgb(32, 172, 149)" prepend-icon="mdi-plus" rounded="lg" variant="flat" @click="addToList">
            add to list
          </v-btn>
        </v-card-actions>
      </v-window-item>

      <v-window-item value="Trade History">
        <template v-for="(c, i) in cards" :key="i">
          <v-card color="transparent" flat>
            <v-card-title class="px-0 font-weight-bold">{{ c.filled }}/USDT</v-card-title>
            <v-card-subtitle class="px-0">
              <v-chip :color="c.short ? 'rgb(32, 172, 149)' : 'rgb(226, 70, 74)'" rounded="lg" size="small"
                      variant="outlined">
                Close {{ c.short ? 'Short' : 'Long' }}
              </v-chip>
              <v-chip class="ml-2" rounded="lg" size="small" variant="outlined">{{ c.status }}</v-chip>
              <v-chip class="ml-2" rounded="lg" size="small" variant="outlined">{{ c.multiple }}X</v-chip>
            </v-card-subtitle>

            <v-row class="mt-3" no-gutters>
              <v-col class="d-flex align-center" cols="12">
                <v-card-text class="text-grey pa-2 px-0">
                  Filled({{ c.filled }})
                </v-card-text>
                <v-spacer/>
                <span>{{ c.value }}</span>
              </v-col>

              <v-col class="d-flex align-center" cols="12">
                <v-card-text class="text-grey pa-2 px-0">
                  Price
                </v-card-text>
                <v-spacer/>
                <span>{{ c.price }}</span>
              </v-col>

              <v-col class="d-flex align-center" cols="12">
                <v-card-text class="text-grey pa-2 px-0">
                  Realized P&L(USDT)
                </v-card-text>

                <v-spacer/>

                <span :style="`color: ${c.realized.indexOf('-') >= 0 ? 'rgb(226, 70, 74)' : 'rgb(32, 172, 149)'}`">
                  {{ c.realized }}
                </span>
              </v-col>

              <v-col class="d-flex align-center" cols="12">
                <v-card-text class="text-grey pa-2 px-0">
                  Trading Fee(USDT)
                </v-card-text>
                <v-spacer/>
                <span>{{ c.tradingFee }}</span>
              </v-col>

              <v-col class="d-flex align-center" cols="12">
                <v-card-text class="text-grey pa-2 px-0">
                  {{ c.date }}
                </v-card-text>
                <v-spacer/>
                <v-icon v-show="c.icon">mdi-tray-arrow-up</v-icon>
              </v-col>
            </v-row>
          </v-card>

          <v-divider class="mt-2"/>
        </template>
      </v-window-item>

      <v-window-item value="Transaction History">
        <v-text-field v-model="deleteIndex" label="delete item"/>

        <v-card-actions>
          <v-btn :disabled="deleteIndex < 0" variant="flat" @click="deleteItem">
            delete
          </v-btn>
        </v-card-actions>
      </v-window-item>
    </v-window>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      tab: 'Trade History',
      items: [
        'Order History', 'Trade History', 'Transaction History'
      ],
      obj: {
        short: true,
        status: 'Isolated',
        multiple: 5,
        filled: 'MKR',
        value: '0.017',
        price: '1,101.3',
        realized: '-3.7077',
        tradingFee: '-0.0112',
        date: '2023-07-29 11:04:05',
        icon: false
      },
      temp: {
        short: true,
        status: 'Isolated',
        multiple: 5,
        filled: 'MKR',
        value: '0.017',
        price: '1,101.3',
        realized: '-3.7077',
        tradingFee: '-0.0112',
        date: '2023-07-29 11:04:05',
        icon: false
      },
      cards: [
        {
          short: true,
          status: 'Isolated',
          multiple: 5,
          filled: 'MKR',
          value: '0.017',
          price: '1,101.3',
          realized: '-3.7077',
          tradingFee: '-0.0112',
          date: '2023-07-29 11:04:05',
          icon: false
        }, {
          short: true,
          status: 'Cross',
          multiple: 10,
          filled: 'XRP',
          value: '1.472',
          price: '0.6793',
          realized: '-3.6799',
          tradingFee: '-0.5018',
          date: '2023-07-25 11:48:19',
          icon: true
        }, {
          short: false,
          status: 'Cross',
          multiple: 30,
          filled: 'TRX',
          value: '37,098',
          price: '0.08047',
          realized: '11.8713',
          tradingFee: '-1.4986',
          date: '2023-07-24 19:46:41',
          icon: true
        }, {
          short: false,
          status: 'Cross',
          multiple: 30,
          filled: 'TRX',
          value: '37,122',
          price: '0.08056',
          realized: '15.2200',
          tradingFee: '-1.5029',
          date: '2023-07-24 15:05:24',
          icon: true
        }, {
          short: false,
          status: 'Cross',
          multiple: 5,
          filled: 'LINA',
          value: '130,970',
          price: '0.01298',
          realized: '-83.3189',
          tradingFee: '-0.8083',
          date: '2023-07-24 13:30:17',
          icon: true
        }, {
          short: false,
          status: 'Cross',
          multiple: 5,
          filled: 'LINA',
          value: '150,489',
          price: '0.01329',
          realized: '-32.2741',
          tradingFee: '-0.9839',
          date: '2023-07-24 01:11:50',
          icon: true
        }, {
          short: false,
          status: 'Isolated',
          multiple: 30,
          filled: 'LTC',
          value: '33.3',
          price: '92.05',
          realized: '1.3320',
          tradingFee: '-1.5333',
          date: '2023-07-23 17:31:33',
          icon: true
        }, {
          short: true,
          status: 'Cross',
          multiple: 5,
          filled: 'ICP',
          value: '467.07',
          price: '4.282',
          realized: '-5.2789',
          tradingFee: '-1.0026',
          date: '2023-07-23 15:35:47',
          icon: true
        },
      ],
      deleteIndex: -1
    }
  },

  mounted() {
    let c = localStorage.getItem('cards');

    if (c) {
      this.cards = JSON.parse(c)
    }
  },

  methods: {
    addToList() {
      let c = this.cards
      c.unshift(this.obj)
      localStorage.setItem('cards', JSON.stringify(c))

      this.obj = this.temp
      this.tab = 1
    },
    deleteItem() {
      let index = this.deleteIndex >= this.cards.length
        ? this.cards.length - 1
        : this.deleteIndex < 0 ? 0 : this.deleteIndex

      this.cards.splice(index, 1)

      localStorage.setItem('cards', JSON.stringify(this.cards))
      this.deleteIndex = -1
    }
  }
}
</script>
