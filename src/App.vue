<script>
import WeatherCard from './components/WeatherCard.vue';

export default{
  data() {
    return {
      locationIndexArr: [5, 4, 13, 11, 9, 6, 15, 17],
      //台北市5 新竹市4 桃園市13 台中市11 雲林9 台南6 高雄15 屏東17
      weatherInfo: [],
    }
  },
  components:{
    WeatherCard,
  },
  methods: {
    getWeatherData(){
      fetch("https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-C0032-001?Authorization=CWB-D152C3EB-4FD1-4CFB-9DFE-2211DA455FBE")
        .then(res => res.json())
        .then(data => {
          console.log(data);
          /* 
          需要的資料：

          1.縣市名稱：
          2.天氣描述：
          3.最低溫度：
          4.最高溫度：
          5.天氣icon

          每個縣市的資料用一個物件包起來，
          push進locationIndexArr，
          再v-for出來
          */
          this.locationIndexArr.forEach((item, index) =>{
            const locationName = data.records.location[item].locationName;

            let parameterName = "";
            //若天氣描述字數過多，則插入換行字元
            if(data.records.location[item].weatherElement[0].time[0].parameter.parameterName.length > 4){
              let wordBreak = data.records.location[item].weatherElement[0].time[0].parameter.parameterName;
              wordBreak = wordBreak.slice(0, 4) + "\n" + wordBreak.slice(4);
              parameterName = wordBreak;
            }else{
              parameterName = data.records.location[item].weatherElement[0].time[0].parameter.parameterName;
            }
            



            const minT = data.records.location[item].weatherElement[2].time[0].parameter.parameterName;
            const maxT = data.records.location[item].weatherElement[4].time[0].parameter.parameterName;

            let weatherIcon = "";

            if(parameterName.includes("多雲")){
              weatherIcon = `<i class="fa-solid fa-cloud"></i>`;
            }
            if(parameterName.includes("陣雨")){
              weatherIcon = `<i class="fa-solid fa-cloud-showers-heavy"></i>`;
            }
            if(parameterName == "晴時多雲" ||
              parameterName == "多雲時晴"){
              weatherIcon = `<i class="fa-solid fa-cloud-sun"></i>`;
            }
            if(parameterName.includes("雷")){
              weatherIcon = `<i class="fa-solid fa-cloud-bolt"></i>`;
            }
            if(parameterName.includes("晴") && parameterName.includes("陣雨")){
              weatherIcon = `<i class="fa-solid fa-cloud-sun-rain"></i>`;
            }

            this.weatherInfo.push({
              locationName,
              parameterName,
              minT,
              maxT,
              weatherIcon,
            });
          });
        });
    },

  },
  mounted(){
    this.getWeatherData();
    console.log(this.weatherInfo)
  }
}
</script>

<template>

  <!-- fontawesome用 -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" integrity="sha512-iecdLmaskl7CVkqkXNQ/ZH/XLlvWZOJyj7Yy7tcenmpD1ypASozpmT/E0iPtmFIB46ZmdtAc9eNBvH0H/ZpiBw==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <!-- fontawesome用 -->

  <!-- 想對地圖本身做特效的話可以參考這個?
  https://jajaaan.co.jp/web-production/frontend/svg-background-image/ -->


  <div class="container flex flex-wrap h-3/4 justify-between px-5 items-center mx-auto my-12 max-w-4xl">
    <WeatherCard
    v-for="info in weatherInfo"
    :location-name="info.locationName"
    :parameter-name="info.parameterName"
    :minT="info.minT"
    :maxT="info.maxT"
    >
    <!-- 待會fontawesome寫在這裡 -->
    <div class="absolute top-7 right-8 text-4xl" v-html="info.weatherIcon"></div>
    </WeatherCard>

  </div>
</template>

<style scoped>

</style>
