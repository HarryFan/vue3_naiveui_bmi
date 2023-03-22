<template>
  <div>
    <el-row>
      <el-col :span="6">
        <el-input v-model="height" placeholder="請輸入身高(cm)"></el-input>
      </el-col>
      <el-col :span="6">
        <el-input v-model="weight" placeholder="請輸入體重(kg)"></el-input>
      </el-col>
      <el-col :span="6">
        <el-button type="primary" @click="calculateBMI">計算BMI</el-button>
      </el-col>
    </el-row>
    <el-row v-if="bmiResult">
      <el-col :span="12">
        <h3>BMI指數：{{ bmiResult }}</h3>
      </el-col>
      <el-col :span="12">
        <h3>評估結果：{{ bmiStatus }}</h3>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      height: '',
      weight: '',
      bmiResult: '',
      bmiStatus: '',
    };
  },
  methods: {
    calculateBMI() {
      const heightInMeter = this.height / 100;
      const bmi = (this.weight / (heightInMeter * heightInMeter)).toFixed(2);
      this.bmiResult = bmi;
      if (bmi < 18.5) {
        this.bmiStatus = '體重過輕';
      } else if (bmi >= 18.5 && bmi < 24) {
        this.bmiStatus = '正常範圍';
      } else if (bmi >= 24 && bmi < 27) {
        this.bmiStatus = '過重';
      } else if (bmi >= 27 && bmi < 30) {
        this.bmiStatus = '輕度肥胖';
      } else if (bmi >= 30 && bmi < 35) {
        this.bmiStatus = '中度肥胖';
      } else {
        this.bmiStatus = '重度肥胖';
      }
    },
  },
};
</script>
