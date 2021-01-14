<template>
  <h2>Daily Sales</h2>

  <div id="chart"></div>
</template>

<script lang="ts">
import {onMounted} from 'vue';
import axios from 'axios';
import * as c3 from 'c3';

export default {
  name: "Dashboard",
  setup() {
    onMounted(async () => {
      const chart = c3.generate({
        bindto: '#chart',
        data: {
          x: 'x',
          columns: [
            ['x'],
            ['Sales']
          ],
          types: {
            Sales: 'bar'
          }
        },
        axis: {
          x: {
            type: 'timeseries',
            tick: {
              format: '%Y-%m-%d'
            }
          }
        }
      });

      const {data} = await axios.get('chart');

      chart.load({
        columns: [
          ['x', ...data.map((r: any) => r.date)],
          ['Sales', ...data.map((r: any) => r.sum)]
        ]
      })
    });
  }
}
</script>
