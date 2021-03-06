<template lang="pug">
  .container
    data-table(
      :payload='data',
      :columns='columns',
      :configuration='configuration'
    )
      template(slot='slot-title')
        div(style="margin-bottom: 0.75rem;")
          h1.is-size-2
            strong Example #2 - <a href="https://iextrading.com/">IEX Developer Platform</a> Data-Table
          p This data is pulled from <code>https://api.iextrading.com/1.0/stock/market/batch?symbols={symbols}&amp;types=quote&amp;range=1m&amp;last=5</code> when the page is loaded &amp; refreshed.
          p This data-table features currency formatting &amp; hidden columns
      div(slot='name', slot-scope="data")
        template(v-if='data.item.logo.length')
          span {{ data.item.name }}
          figure.image.is-32x32
            img(:src='data.item.logo')
        span(v-else='') {{ data.item.name }}
</template>

<script>
import Axios from 'axios'
import _ from 'lodash'
import DataTable from 'vstx-data-table'

export default {
  name: 'example-2-data-table',
  components: {
    'data-table': DataTable
  },
  data () {
    return {
      data: [],
      columns: [
        {
          name: 'Company Name',
          field: 'companyName',
          isVisible: true,
          sort: {
            isSortable: true,
            direction: 'asc',
            order: 1
          }
        },
        {
          name: 'Stock',
          field: 'symbol',
          isVisible: true,
          sort: {
            isSortable: false
          }
        },
        {
          name: 'Sector',
          field: 'sector',
          isVisible: true,
          sort: {
            isSortable: false
          }
        },
        {
          name: 'Price',
          field: 'latestPrice',
          format: 'formatMoney',
          isVisible: true,
          sort: {
            isSortable: true,
            direction: 'desc',
            order: 0
          }
        },
        {
          name: 'Market Cap',
          field: 'marketCap',
          format: 'formatMoney',
          isVisible: true,
          sort: {
            isSortable: false
          }
        }
      ],
      configuration: {
        table: {
          bordered: true,
          striped: true,
          hoverable: true,
          fullwidth: true
        },
        filter: {
          isVisible: true,
          isAllowed: true,
          isEvent: false
        },
        columns: {
          isVisible: false,
          isAllowed: true
        },
        pagination: {
          rowsPerPage: 25
        }
      }
    }
  },
  created () {
    this.getData('https://api.iextrading.com/1.0/stock/market/batch?symbols=AAPL,GOOGL,GOOG,MSFT,FB,TSM,INTC,ORCL,CSCO,IBM,SAP,NVDA,TXN,AVGO,QCOM,ADBE,DCM,CRM,BIDU,AABA,AMAT,ITW,MU,VMW,ADP,ATVI,CTSH,INTU,NXPI,ETN,HPQ,INFY,EA,ADI,LRCX,DXC,FISV,NOK,WIT,KYO,WDC,ADSK,HPE,CERN,NOW,WPP,WDAY,RHT,MCHP,STM,XLNX,SWKS,INFO,SYMC,TWTR,SNAP,ANET,CHKP,DVMT,OMC,SQ,VRSK,DOV,LLL,NTAP,MFGP,MXIM,MSI,URI,CA,PANW,CTXS,TTWO,SNPS,PNR,ASML,IPGP,ANSS,SPLK,STX,CDNS,TEAM,ASX,AMD,MRVL,YNDX,VRSN,JNPR,FLEX,LDOS,ON,DOX,SHOP,LN,JKHY,OTEX,ATHM,MTCH,QRVO&types=quote&range=1m&last=5')
  },
  methods: {
    getData (url, method = 'get', fomatter = this.formatData) {
      Axios(
        {
          method: method,
          url: url,
          responseType: 'json'
        })
        .then((res) => fomatter(res))
    },
    formatData (res) {
      console.log(res.data['AAPL']['quote'])
      return _.forEach(res.data, (obj, key) => {
        this.data.push(obj['quote'])
      })
    }
  }
}
</script>

<style>
  .data-table .table {
    margin-top: 0.5rem;
  }
</style>
