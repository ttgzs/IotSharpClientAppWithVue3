<template>
  <div>
    <el-drawer v-model="drawer" :title="dialogTitle" size="70%">
      <el-tabs v-model="activeTabName" class="demo-tabs" stretch>
        <el-tab-pane label="详情" name="basic">
          <div class="z-tab-container">
            <AdvancedKeyValue
                :obj="deviceRef"
                :config="columns"
                :hide-key-list="['owner', 'identityValue', 'tenantName', 'customerName', 'tenantId', 'customerId']"
                :label-width="160"></AdvancedKeyValue>
          </div>
        </el-tab-pane>
        <el-tab-pane label="属性" name="props">
          <div class="z-tab-container">
            <DeviceDetailProps :deviceId="deviceRef.id"></DeviceDetailProps>
          </div>

        </el-tab-pane>
        <el-tab-pane label="遥测" name="telemetry">
          <div class="z-tab-container">
            <DeviceDetailTelemetry :deviceId="deviceRef.id"></DeviceDetailTelemetry>
          </div>
        </el-tab-pane>
        <el-tab-pane label="告警" name="alarm">
          <div class="z-tab-container">
            <alarmlist :originator="deviceRef.id" wrapper="div"></alarmlist>
          </div>
        </el-tab-pane>
        <el-tab-pane label="规则" name="rules">
          <div class="z-tab-container">
            <DeviceDetailRules :deviceId="deviceRef.id"></DeviceDetailRules>
          </div>
        </el-tab-pane>
        <el-tab-pane label="规则历史" name="rulesHistory">
          <div class="z-tab-container">
            <flowevents :creator="deviceRef.id"  wrapper="div"></flowevents>
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-drawer>
  </div>
</template>
<script lang="ts" setup>
import { createDeviceCrudOptions } from "/@/views/iot/devices/deviceCrudOptions";
import AdvancedKeyValue from "/@/components/AdvancedKeyValue/AdvancedKeyValue.vue";
import DeviceDetailProps from "/@/views/iot/devices/detail/DeviceDetailProps.vue";
import DeviceDetailRules from "/@/views/iot/devices/detail/DeviceDetailRules.vue";
import DeviceDetailTelemetry from "/@/views/iot/devices/detail/DeviceDetailTelemetry.vue";
import Alarmlist from "/@/views/iot/alarms/alarmlist.vue";
import Flowevents from "/@/views/iot/rules/flowevents.vue";

const drawer = ref(false);
const dialogTitle = ref(`设备详情`);
const activeTabName = ref('basic')
const deviceRef = ref()
const {crudOptions: {columns}} = createDeviceCrudOptions({expose: null})
const openDialog = (device: any) => {
  drawer.value = true
  deviceRef.value = device
  dialogTitle.value = `${deviceRef.value.name}设备详情`
};
defineExpose({
  openDialog,
});

</script>
<style lang="scss" scoped>
.z-tab-container {
  padding: 0 16px 16px;
}
</style>
