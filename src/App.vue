<template>
  <div>
    Select Application:
    <button
      @click="which = it"
      v-for="(it,key) in items"  v-text="it" :class="which === it ? 'active' : ''"/>
    <br>

    Windows {{which}}
   <div class="windows">
     <DynamicComponent :key="currentComponent" :link="currentComponent" />
   </div>
  </div>
</template>

<script>
import DynamicComponent from "./components/DynamicComponent.vue";
export default {
  components: {DynamicComponent},
  data(){
    return{
      items: ['1', '2', '3', '4'],
      which:'1',
    }
  },
  computed:{
    currentComponent(){
      return this.which;
    }
  }
}
</script>

<style>
button{
  margin: 1rem;
  padding: .5rem 2rem;
}
.active{
  background: lightgray;
}
.windows{
  margin-top: 2rem;
  border: 1px solid black;
  padding: 1rem;
}
</style>

