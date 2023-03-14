<template>
  <div class="bmi-container">
    <!-- 身高輸入框 -->
    <n-input-number v-model:value="height" name="height" placeholder="身高(cm)" clearable />
    <!--  體重輸入框 -->
    <n-input-number v-model:value="weight" name="weight" placeholder="體重(kg)" clearable />
    <!-- 計算BMI按鈕 -->
    <n-button @click="calculateBMI">計算BMI</n-button>
    <!-- 當bmiResult有值時才顯示n-result -->
    <!-- 顯示BMI指數的結果標題和內容 -->
    <n-result v-if="bmiResult" :status="bmiResult.status" :title="bmiResult.title" :description="bmiResult.content" />
  </div>
</template>
<style scoped>
  .bmi-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 16px;
  }
</style>
<script>
  import {
    defineComponent,
    ref
  } from 'vue';
  import {
    NInputNumber,
    NButton,
    NResult
  } from 'naive-ui';

  export default defineComponent({
    name: 'BMIComponent',
    components: {
      NInputNumber,
      NButton,
      NResult,
    },
    setup() {
      // 設定身高、體重和BMI指數的狀態
      const height = ref(null);
      const weight = ref(null);
      const bmiResult = ref(null);

      // 計算BMI指數的函式
      const calculateBMI = () => {
        console.log("height = " + height.value + ", weight = " + weight.value);

        // 判斷身高和體重是否符合規範，如果符合則計算BMI指數，否則顯示錯誤訊息
        if (height.value && weight.value && height.value > 0 && weight.value > 0) {
          const bmi = parseFloat(weight.value) / Math.pow(parseFloat(height.value) / 100, 2);
          // 直接設定bmiResult的值
          bmiResult.value = getBmiResult(bmi);
        } else {
          bmiResult.value = {
            title: '輸入錯誤',
            content: '請輸入正確的"身高"和"體重"。',
          };
        }
      };

      // 根據BMI指數的值返回對應的標題和內容  
      const getBmiResult = (bmi) => {
        let status = '';
        let title = '';
        let content = '';
        if (bmi < 18.5) {
          status = 'warning';
          title = '過輕';
          content = '您的體重過輕，請增加飲食攝取量。';
        } else if (bmi < 24) {
          status = 'success';
          title = '正常';
          content = '您的體重正常，請繼續保持。';
        } else if (bmi < 27) {
          status = 'warning';
          title = '過重';
          content = '您的體重過重，請減少高熱量食物和增加運動量。';
        } else if (bmi < 30) {
          status = 'warning';
          title = '輕度肥胖';
          content = '您已經達到輕度肥胖的程度，請控制飲食並增加運動。';
        } else if (bmi < 35) {
          status = 'error';
          title = '中度肥胖';
          content = '您已經達到中度肥胖的程度，請尋求專業的醫學指導。';
        } else {
          status = 'error';
          title = '極度肥胖';
          content = '您已經達到極度肥胖的程度，請立即尋求專業的醫學指導。';
        }
        return {
          status,
          title,
          content,
        };
      };
      return { // 返回height、weight和bmiResult三個狀態和calculateBMI和getBmiResult兩個函數
        height,
        weight,
        bmiResult,
        calculateBMI,
      };
    },
  });
</script>
<!-- 
這是一個計算BMI指數的元件程式碼，使用Vue.js框架和Naive UI組件庫。
它包括了以下幾個部分：

模板區塊（template）：定義了使用者界面的結構和組件，包括了兩個輸入框（n-input-number）、一個按鈕（n-button）和一個結果框（n-result）。

样式區塊（style scoped）：定義了元件內部使用的CSS樣式，具有局部作用域。

腳本區塊（script）：使用Vue.js的defineComponent方法定義了一個名為BMIComponent的元件。
setup函數中定義了height、weight和bmiResult三個狀態，以及calculateBMI和getBmiResult兩個函數。
calculateBMI函數通過height和weight狀態的值計算出BMI指數，然後根據BMI指數的值顯示不同的結果。
getBmiResult函數根據BMI指數的值返回對應的標題和內容。

用return返回了height、weight、bmiResult和calculateBMI這四個對象，這些對象可以在模板中使用。

其中，v-model指令用於雙向綁定輸入框的值和狀態；@click指令用於綁定按鈕點擊事件；v-show指令用於控制結果框的顯示和隱藏；v-if指令也用於控制結果框的顯示和隱藏，不同的是v-show只是控制顯示和隱藏，而v-if則是動態創建和銷毀元素，可以節省DOM渲染開銷。
此外，使用了ref和parseFloat等Vue.js中的常用方法和API。 
-->