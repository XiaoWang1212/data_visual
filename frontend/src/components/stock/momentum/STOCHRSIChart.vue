<template>
  <div class="stochrsi-chart">
    <h3>{{ symbol }} STOCHRSI Chart</h3>
    <p v-if="loading" class="loading">Loading chart data...</p>
    <p v-if="error" class="error">{{ error }}</p>
    <div v-if="chartData" ref="chartContainer" class="chart-container"></div>
    <div class="info-container" @click="showInfo = !showInfo">
      <span class="material-icons info-icon">info</span>
      <span class="info-text">什麼是隨機相對強弱指標?</span>
    </div>
    <div v-if="showInfo" class="info-box">
      <p>隨機相對強弱指標(STOCHRSI)</p>
      <p>
        隨機相對強弱指標，結合RSI指標與STOCH指標，利用收盤價計算每日K棒的RSI值，並參考隨機指標計算方式，將RSI指標轉換為隨機相對強弱指標，數值同樣介於0~100之間
      </p>
      <p>
        依據STOCHRSI可判斷股市買賣情形，當數值超過80即為超買，低於20即為超賣，提醒投資者小心極端的市場買賣情形
      </p>
      <img src="@/assets/photos/STOCHRSI.png" alt="STOCHRSI Photo" />
    </div>
  </div>
</template>

<script>
  import Plotly from "plotly.js-dist";
  import { mapState, mapActions } from "vuex";
  import { nextTick } from "vue";

  export default {
    props: {
      symbol: {
        type: String,
        required: true,
      },
    },
    data() {
      return {
        chartData: null,
        showInfo: false,
      };
    },
    computed: {
      ...mapState("stockApp", {
        loading: (state) => state.loading,
        error: (state) => state.error,
      }),
    },
    watch: {
      symbol: "fetchChartData",
    },
    methods: {
      ...mapActions("stockApp", ["fetchSMAChartData"]),
      async fetchChartData() {
        this.chartData = null;
        await this.fetchSMAChartData(this.symbol);
        if (!this.error) {
          this.chartData = this.$store.state.stockApp.chartData;
          await nextTick(); // 確保 DOM 更新完成
          this.renderChart();
        }
      },
      async renderChart() {
        await nextTick(); // 確保 DOM 更新完成
        const { dates, stochrsi_fastk, stochrsi_fastd } = this.chartData;

        const traceK = {
          x: dates,
          y: stochrsi_fastk,
          type: "scatter",
          mode: "lines",
          name: "K值",
          line: { color: "green" },
        };

        const traceD = {
          x: dates,
          y: stochrsi_fastd,
          type: "scatter",
          mode: "lines",
          name: "D值",
          line: { color: "red" },
        };

        const layout = {
          title: `${this.symbol} STOCHRSI Chart`,
          xaxis: { title: "Date" },
          showlegend: true,
        };

        Plotly.newPlot(
          this.$refs.chartContainer,
          [traceK, traceD],
          layout
        );
      },
    },
    mounted() {
      this.fetchChartData();
    },
  };
</script>

<style scoped>
  .stochrsi-chart {
    margin-top: 20px;
    position: relative;
  }

  .chart-container {
    position: relative;
  }

  .loading {
    color: #007bff;
  }

  .error {
    color: #ff0000;
  }

  .info-container {
    cursor: pointer;
    display: flex;
    align-items: center;
    position: absolute;
    top: 10px;
    right: 160px;
  }

  .info-icon {
    font-size: 24px;
    margin-right: 5px;
  }

  .info-text {
    font-size: 14px;
    color: #007bff;
  }

  .info-box {
    background-color: #f9f9f9;
    border: 1px solid #ccc;
    padding: 10px;
    position: absolute;
    top: 40px;
    right: 10px;
    width: 300px;
    z-index: 1000;
  }

  .info-box img {
    max-width: 100%;
    height: auto;
  }
</style>
