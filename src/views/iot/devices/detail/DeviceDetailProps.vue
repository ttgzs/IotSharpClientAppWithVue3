<template>
  <div class="z-crud">
    <fs-crud ref="crudRef" v-bind="crudBinding"/>
  </div>
</template>

<script setup lang="ts">
import {useCrud} from "@fast-crud/fast-crud";
import {useExpose} from "@fast-crud/fast-crud";
import {createDevicePropsCrudOptions} from "/@/views/iot/devices/detail/devicePropsCrudOptions";
const props = defineProps({
  deviceId: {
    type: String,
    default: ''
  }
})
// crud组件的ref
const crudRef = ref();
// crud 配置的ref
const crudBinding = ref();
// 暴露的方法
const {crudExpose} = useExpose({crudRef, crudBinding});
// 你的crud配置
let {crudOptions} = createDevicePropsCrudOptions({expose: crudExpose}, props.deviceId);
// 初始化crud配置
// eslint-disable-next-line @typescript-eslint/no-unused-vars,no-unused-vars
const {resetCrudOptions} = useCrud({expose: crudExpose, crudOptions});
// 你可以调用此方法，重新初始化crud配置
// resetCrudOptions(options)
watch(() => props.deviceId, () => {
  // watch deviceId , 根据Device id 重新配置 crud， 再进行刷新
  const res =  createDevicePropsCrudOptions({expose: crudExpose}, props.deviceId);
  crudOptions = res.crudOptions
  // console.log(`%c@DeviceDetailProps:32`, 'color:white;font-size:16px;background:green;font-weight: bold;', res)
  resetCrudOptions(crudOptions)
  crudExpose.doRefresh();
})
// 页面打开后获取列表数据
onMounted(() => {
  crudExpose.doRefresh();
});

</script>

<style scoped lang="scss">
.z-crud {
  height: calc(100vh - 160px);
}
</style>
