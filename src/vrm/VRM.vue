<script setup lang="ts">
import {onBeforeMount, onMounted, ref} from "vue";
import {useStore} from "vuex";
import VRMDrawer from "./VRMDrawer.vue";
import viewer from "./loadVRM";
import {useRoute} from "vue-router";


const canvas:any= ref(null)

const store = useStore()

onBeforeMount(()=>{
  const loading = ElLoading.service({ fullscreen: true })
  store.commit("setLoading",loading)
})

onMounted(async ()=>{


  const route = useRoute()
  const modelName = route.params.modelName
  if (canvas) {
    viewer.setup(canvas.value);
    await viewer.loadVrm("/model/"+modelName);
    setTimeout(()=>{
      store.commit("closeLoading")
    },1000)
  }

})

</script>

<template>
  <VRMDrawer></VRMDrawer>
  <canvas ref="canvas"></canvas>

</template>

<style scoped>
canvas {
  margin: 0;
  padding:0;
}
</style>