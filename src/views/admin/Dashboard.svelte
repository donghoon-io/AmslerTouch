<script>
  // core components
  import CardLineChart from "components/Cards/CardLineChart.svelte";
  import CardBarChart from "components/Cards/CardBarChart.svelte";
  import CardPageVisits from "components/Cards/CardPageVisits.svelte";
  import CardSocialTraffic from "components/Cards/CardSocialTraffic.svelte";
  import Test from "views/Test.svelte";
  export let location;
</script>

<div>
  <div class="flex flex-wrap"> 
    <Test />
  </div>
</div>
