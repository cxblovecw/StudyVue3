<template>
  <div>
    <h4>演示computed和watch</h4>
    <ul>
      <li id="priceItem">价格:{{priceDes}} <button @click="price++">加价</button></li>
    </ul>
  </div>
</template>

<script>
import { computed,ref, watch, watchEffect } from 'vue';
export default {
  setup(){
    let price=ref(0);
    // 1.computed 计算属性 当依赖发生改变时 会触发页面更新(响应式)
    let priceDes=computed(()=>{
      return price.value+"元"
    })
    // 2.watchEffect 在响应式地跟踪其依赖项时立即运行一个函数，并在更改依赖项时重新运行它
    let stopWatchEffect=watchEffect(()=>{
      console.log('price发生改变,现在的值为:',price.value);
      console.log(document.getElementById("priceItem"));
    },{
      // flush 决定watchEffect的函数\
      // 在DOM渲染前执行(pre)     首次渲染拿不到DOM元素 
      // 在DOM渲染后执行(post)    首次渲染拿的到DOM元素
      // 和DOM渲染同步执行(sync)  首次渲染拿不到DOM元素
      flush:"sync",
      onTrack(){console.log('跟踪依赖');},
      onTrigger(){console.log('触发更新')}
    })
    // stopWatchEffect();  // 停止监听

    // 3.watch
    watch(
      ()=>price.value,
      (price,prePrice)=>{
        console.log('新的价格:',price);
        console.log('之前价格:',prePrice);
    })
    // 一次性可以监听多个数据源
    // let fooRef=ref('foo');
    // let barRef=ref('bar');
    // watch([fooRef, barRef], ([foo, bar], [prevFoo, prevBar]) => {
    // })
    return {price,priceDes}
  }
}
</script>

<style>

</style>