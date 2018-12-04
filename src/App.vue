<template>
  <div id="app">
    <div class="wrap">
      <textarea name="" id=""  class="box1" cols="30" rows="10" v-model="value1"></textarea>
      <select name="" id="" class="select-box"  v-model="typesCompute">
        <option value="+">+</option>
        <option value="-">-</option>
      
      </select>
       <textarea name="" id=""  class="box1" cols="30" rows="10" v-model="value2"></textarea>
       <button @click="handleCompute()">计算</button>
       <p>结果：{{ sum }}</p>
    </div>
   
   
  </div>
</template>

<script>
export default {
  data(){
    return {
      sum:"",
      value1:"1",
      value2:"2",
      typesCompute:'+'
    }
  },
 
  methods:{
     handleCompute(){
     
      if(isNaN(this.value1) ||isNaN(this.value2)){
         alert('必须输入数字'); 
         return;
      }
     
      switch(this.typesCompute){
        case '+':
          this.add(this.value1,this.value2);
          break;
        case '-':
          this.reduce(this.value1,this.value2);
          break;
       
        
      }
       
    },
    add(num1,num2){
      let negative = 0;
      if(num1[0] == '-'){
          negative++;
          // num1 = num1.replace(/-/g, '');
      }if(num2[0] == '-'){
          negative++;
          // num2 = num2.replace(/-/g, '');
      }
      if(negative === 1 && num1[0] =='-'){
        this.reduce(num2, num1.replace(/-/g, ''))
        return false
      }
      if(num1[0] == '-'){
        
          num1 = num1.replace(/-/g, '');
      }if(num2[0] == '-'){
         
          num2 = num2.replace(/-/g, '');
      }
      let arrNum1 = num1.split('').reverse();
      let arrNum2 = num2.split('').reverse();
      let carry = 0;
      let tempSum = 0;
      let reserveSum = '';
      let len = arrNum1.length > arrNum2.length ? arrNum1.length : arrNum2.length ;
      for(let i=0; i<len; i++){
        if(arrNum1[i] && arrNum2[i]){
            tempSum = Number(arrNum1[i]) + Number(arrNum2[i]) + carry;
        }else if(arrNum1[i]){
            tempSum = Number(arrNum1[i]) + carry;
        }else{
            tempSum = Number(arrNum2[i]) + carry;
        }

        reserveSum += ( tempSum >= 10 ? tempSum - 10 : tempSum) ;
        carry =  tempSum >= 10 ? 1 : 0;
       
      }

      if(carry != 0){
        reserveSum += carry;
      }
      this.sum  = String(negative === 2 ? '-':'')+ reserveSum.split('').reverse().join('');


    },
    isCompare(num1,num2){
       return num1-num2 < 0 ?true : false;
    },
    reduce(num1,num2){
        let negative = 0;
        let arrNum1 = num1.split('').reverse();
        let arrNum2 = num2.split('').reverse();
        let carry = 0;
        let tempSum = 0;
        let reserveSum = '';
        if(num1[0] == '-'){
            negative++;
        }
        if(num2[0] == '-'){
            negative++;
        }
        if(this.isCompare(num1,num2)){
          //1-2
          this.reduce(num2,num1);
          return false;
        }
        if(negative === 1 && num1[0] == '-'){
          //  -3-3
           this.add(num1,'-'+num2);
           return false;
        }
         if(negative === 1 && num2[0] == '-'){
        //  3-(-3)
           this.add(num1,num2.replace(/-/g, ''));
           return false;
        }
        let len = arrNum1.length > arrNum2.length ? arrNum1.length : arrNum2.length ;
        for(let i=0; i<len; i++){
          if(arrNum1[i] && arrNum2[i]){
              tempSum = Number(arrNum1[i]) - Number(arrNum2[i]) + carry;
          }else if(arrNum1[i]){
              tempSum = Number(arrNum1[i]) + carry;
          }else{
              tempSum = Number(arrNum2[i]) + carry;
          }

        reserveSum += ( tempSum < 0 ? tempSum + 10 : tempSum) ;
        if(tempSum < 0){
           carry = -1;
        }else{
          carry = 0;
        }
       
      }

      if(this.isCompare(num1,num2)){
          this.sum = '-'+reserveSum.split('').reverse().join('');
      }else{
          this.sum  = String(negative === 2 ? '-':'')+ reserveSum.split('').reverse().join('');
      }
      this.sum = parseInt(this.sum);
    }
  
  }
}
</script>

<style>
.box1{
  display: inline-block;
  margin: 0 6px;
 }
 .select-box{
   vertical-align: top;
 }



</style>
