<template>
  <div class="chart">
    <p class="chart-title">
      {{ chartData.title }}
    </p>
    <div
      class="chart-item"
      v-for="(value, index) in sortedData"
      :key="`chart-item-${value[0]}`"
    >
      <div class="chart-bar-container">
        <div
          class="chart-bar"
          :style="{
            width: `calc(${(100 / maxValue) * value[1]}% - 40px)`,
            'background-color': colors[index]
          }"
        >
          <p class="chart-label">{{ isNaN(value[0]) ? value[0] : `${value[0]}h` }}</p>
          <div class="chart-percent">
            <p>{{ value[1] }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default defineComponent({
  props: {
    chartData: Object
  },
  setup(props) {
    const maxValue = computed(() => {
      let arr = Object.values(props.chartData.data);
      let max = Math.max(...arr);
      return max;
    });

    const sortedData = Object.entries(props.chartData.data).sort(
      (a, b) => b[1] - a[1]
    );

    const colors = [
      'rgba(0,100,0,0.6)',
      'rgba(0,100,0,0.5)',
      'rgba(0,100,0,0.4)',
      'rgba(0,100,0,0.3)',
      'rgba(0,100,0,0.2)',
      'rgba(0,100,0,0.1)',
      'rgba(0,100,0,0.1)',
      'rgba(0,100,0,0.1)'
    ];

    return {
      maxValue,
      colors,
      sortedData
    };
  }
});
</script>

<style lang="scss">
.chart {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 100vw;
  height: 99%;

  .chart-title {
    font-size: 3rem;
    margin: 0;
    margin: 10px 0 10px 15px;
  }

  .chart-item {
    flex-grow: 1;
    padding: 20px 0;

    .chart-bar-container {
      height: 100%;

      .chart-bar {
        position: relative;
        height: 100%;
        max-height: 80px;
        border-radius: 10px;
        margin: 0 20px;
        box-sizing: border-box;

        .chart-label {
          position: absolute;
          font-size: 50px;
          height: 100%;
          margin: 0 0 0 15px;
          width: 100vw;
        }

        .chart-percent {
          position: absolute;
          font-size: 50px;
          height: 100%;
          left: calc(94vw - 50px);
          width: 70px;
          height: 70px;
          display: block;
          margin: 0 0 0 15px;

          box-sizing: border-box;
          border-radius: 50px;

          p {
            margin: auto;
            display: block;
            width: fit-content;
            height: fit-content;
          }
        }
      }
    }
  }
}
</style>