<template>
  <div class="container">
    <i class="decoration"></i>
    <h3>国内疫情</h3>
    <p class="time">数据更新至 {{ virusInfo.updateTime| time }}</p>
    <div class="virusDataBox">
      <div class="virusData">
        <div class="firstLine">
          <ul>
            <p>现有确诊</p>
            <span>{{ virusInfo.currentConfirmedCount | comma }}</span>
            <p>
              较昨日 <i>{{ virusInfo.currentConfirmedIncr | modifyNumber }}</i>
            </p>
          </ul>
          <ul class="">
            <p>累计死亡</p>
            <span>{{ virusInfo.deadCount | comma }}</span>
            <p>
              较昨日 <i>{{ virusInfo.deadIncr | modifyNumber }}</i>
            </p>
          </ul>
          <ul class="">
            <p>累计确诊</p>
            <span>{{ virusInfo.confirmedCount | comma }}</span>
            <p>
              较昨日 <i>{{ virusInfo.confirmedIncr | modifyNumber }}</i>
            </p>
          </ul>
        </div>
        <div class="secondLine">
          <ul class="">
            <p>累计治愈</p>
            <span>{{ virusInfo.curedCount | comma }}</span>
            <p>
              较昨日 <i>{{ virusInfo.curedIncr | modifyNumber }}</i>
            </p>
          </ul>
          <ul class="">
            <p>累计境外输入</p>
            <span>{{ virusInfo.suspectedCount | comma }}</span>
            <p>
              较昨日 <i>{{ virusInfo.suspectedIncr | modifyNumber }}</i>
            </p>
          </ul>
          <ul class="">
            <p>现存无症状</p>
            <span>{{ virusInfo.seriousCount | comma }}</span>
            <p>
              较昨日 <i>{{ virusInfo.seriousIncr | modifyNumber }}</i>
            </p>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import api from "../../api";

export default {
  created() {
    console.log(this.virusInfo);
    api.getEpidemicData().then(res => {
      this.virusInfo =res.data.results[0]

    });
  },
  data(){
    return{
      virusInfo:{}
    }
  },
  filters: {
    //增加的数字若为非负，则前面加一个 + 号
    modifyNumber(number) {
      return number >= 0 ? "+" + number : number;
    },
    //每千分位加，
    comma(number) {
      return number ? number.toLocaleString() : "";
    },
    //时间戳转换
    time(value) {
      return new Date(value).toLocaleString("chinese", { hour12: false });
    },
  },
};
</script>

<style scoped lang="scss">
.container {
  padding: 0 0.1rem;
  margin-top: 0.1rem;
  font-size: 0.15rem;
  h3 {
    margin-left: 0.08rem;
    border-left: 0.1rem #0ea7af solid;
    padding-left: 0.1rem;
  }
  .time {
    color: #999;
    padding-left: 0.3rem;
    margin-top: 0.1rem;
    font-size: 0.14rem;
    display: flex;
    align-items: center;
    &::before {
      content: "";
      position: absolute;
      left: 0.15rem;
      // top: 0.1rem;
      width: 0.15rem;
      height: 0.15rem;
      box-sizing: border-box;
      background: #0faeb5;
      border-radius: 0.15rem;
      border-color: #cfeff0;
      border: 0.03rem solid #cfeff0;
    }
  }
}
.virusDataBox {
  margin-top: 0.1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
.virusData {
  width: 3.3rem;
  border-radius: 0.2rem;
  box-shadow: 0px 0px 0.04rem #7d7d7d;
  height: 2.2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  .firstLine,
  .secondLine {
    display: flex;
    justify-content: center;
    ul {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      p,
      span {
        margin-bottom: 0.05rem;
      }
      span {
        color: #0ea5ae;
        font-size: 0.2rem;
        font-weight: 800;
      }
      i {
        color: orange;
      }
    }
  }
  .firstLine {
    margin-bottom: 0.1rem;
  }
}
</style>
