<template>
  <div class="hello">
    <div class="search_box">
      <span>Please input address</span>
      <input type="text" v-model.lazy.trim="keyword" class="input" />
    </div>
    <table class="table table_border">
      <thead>
        <tr>
          <td>sno</td>
          <td>sna</td>
          <td>tot</td>
          <td>sarea</td>
          <td>mday</td>
          <td>lat</td>
          <td>lng</td>
          <td>ar</td>
          <td>sareaen</td>
          <td>snaen</td>
          <td>aren</td>
          <td>bemp</td>
          <td>srcUpdateTime</td>
          <td>updateTime</td>
          <td>infoTime</td>
          <td>infoDate</td>
        </tr>
      </thead>
      <tr v-for="data in mergeDatas" :key="data">
        <td>{{ data.sno }}</td>
        <td>{{ data.sna }}</td>
        <td>{{ data.tot }}</td>
        <td>{{ data.sarea }}</td>
        <td>{{ data.mday }}</td>
        <td>{{ data.lat }}</td>
        <td>{{ data.lng }}</td>
        <td>{{ data.ar }}</td>
        <td>{{ data.sareaen }}</td>
        <td>{{ data.snaen }}</td>
        <td>{{ data.aren }}</td>
        <td>{{ data.bemp }}</td>
        <td>{{ data.srcUpdateTime }}</td>
        <td>{{ data.updateTime }}</td>
        <td>{{ data.infoTime }}</td>
        <td>{{ data.infoDate }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      keyword: "",
      totaldata1: [],
      totaldata2: [],
      mergeData: [],
    };
  },
  created() {
    // Simple GET request using fetch
    fetch("https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz")
      .then((response) => response.json())
      .then((data) => (this.totaldata1 = data.retVal));

    fetch(
      "https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json"
    )
      .then((response) => response.json())
      .then((data) => (this.totaldata2 = data));
  },
  computed: {
    mergeDatas() {
      // console.log(this.totaldata1);
      // console.log(this.totaldata2);
      // console.log(Object.values(this.totaldata1));
      // Object.values(this.totaldata1);
      // this.totaldata2 = this.totaldata2.mday.replace("-");
      // alert(this.totaldata2);

      this.totaldata2.forEach((data) => {
        console.log(data);
        data.mday = data.mday.replace(/[-|:]/g, "");
        data.sna = data.sna.replaceAll("YouBike2.0_", "");
      });

      let mergeData = Object.values(this.totaldata1).concat(this.totaldata2);
      return mergeData.filter((data) => data.ar.indexOf(this.keyword) > -1);
    },
  },
};
</script>

<style scoped></style>
