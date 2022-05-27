<template>
    <div>
      <img alt="Vue logo" src="./assets/logo.png">
      <h1>{{count}}</h1>
      <h1>{{double}}</h1>
      <h2>{{greetings}}</h2>
      <h1>X：{{x}},Y：{{y}}</h1>
      <h1 v-if="loading">Loading!...</h1>
      <img v-if="loaded" :src="result.message">
      <!-- <ul>
        <li v-for="number in numbers" :key="number"><h1>{{number}}</h1></li>
      </ul> -->
      <h1>{{person.name}}</h1>
      <button @click="increase">▲+1</button>
      <button @click="updateGreeting">updateGreeting</button>
    </div>
</template>

<script lang="ts">
import useMousePosition from './hooks/useMousePosition'
import useURLLoader from './hooks/useURLLoader';
import {
  ref, 
  computed, 
  reactive, 
  toRefs, 
  onMounted, 
  onUpdated, 
  onRenderTriggered, 
  watch,
} from 'vue';
interface DataProps {
  count:number;
  double:number;
  increase: () => void;
  numbers:number[];
  person: {name ?: string};

}
export default ({
  name: 'App',
  setup() {
    // const count = ref(0);
    // const double = computed(()=> {
    //   return count.value * 2
    // })
    // const increase = () => {
    //   count.value++
    // }
    // const greetings = ref('')
    // const updateGreeting = () => {
    //   greetings.value += "Hellow!"
    // }
    //watch 使用一
    // watch(greetings, (newValue, oldValue) => {
    //   console.log('old',oldValue)
    //   console.log('new',newValue)
    //   document.title = 'updated' + greetings.value
    // })  
      

    onMounted(() => {
      console.log('mounted')
    })
    onUpdated(() => {
      console.log('updated')
    })
    onRenderTriggered((event) => {
      console.log(event, "onRenderTriggered")
    })
    const data: DataProps = reactive({
      count:0,
      increase: () => { data.count++ },
      double: computed(() => data.count * 2),
      numbers: [0, 1, 2 ],
      person: {}
    })
    data.numbers[0] = 5
    data.person.name = "viking"

    const greetings = ref('')
    const updateGreeting = () => {
      greetings.value += "Hello!"
    }
    const {result,loading,loaded} = useURLLoader('https://dog.ceo/api/breeds/image/random')
    //watch 使用二
    watch([greetings, () => data.count], (newValue, oldValue) => {
      console.log('old',oldValue)
      console.log('new',newValue)
      document.title = 'updated' + greetings.value + data.count
    })
    const { x, y } = useMousePosition();
    const refData = toRefs(data)
    return {
      ...refData,
      greetings,
      updateGreeting,
      x,
      y,
      result,
      loading,
      loaded
    }
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
