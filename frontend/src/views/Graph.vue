<template>
  <VCard class="ma-2" subtitle="LED A" width="150" flat border align="center">
    <VCardItem class="pa-0">Turned on </VCardItem>
    <VCardItem class="pa-0"><span class="text-h5 text-primary font-weight-bold">{{ led_A }}</span> </VCardItem>
    <VCardItem class="pa-0"> times</VCardItem>
    <VCardItem>
      <VBtn text="Update" class="ma-1 text-caption" rounded="pill" flat color="secondary" variant="tonal"></VBtn>
    </VCardItem>
  </VCard>
</template>

<script setup>

import { onMounted } from 'vue';
import { useMqttStore } from '@/store/mqttStore'; // Import Mqtt Store
import { storeToRefs } from "pinia";
// VARIABLES
const Mqtt = useMqttStore();
const { payload } = storeToRefs(Mqtt);

onMounted(() => {
  // THIS FUNCTION IS CALLED AFTER THIS COMPONENT HAS BEEN MOUNTED
  Mqtt.connect(); // Connect to Broker located on the backend
  setTimeout(() => {
    // Subscribe to each topic
    Mqtt.subscribe("620164974");
    Mqtt.subscribe("620164974_sub");
  }, 3000);
});





const toggle = (name) => {
  let message = JSON.stringify({ "type": "toggle", "device": name }); // Create message and convert to a json string
  Mqtt.publish("topic", message); // Publish message to appropriate topic
}

</script>


<style scoped>
/** CSS STYLE HERE */
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Symbols+2&display=swap');

.digit {
  font-family: 'Noto Sans Symbols 2';
  font-size: 250px;
}

.container {
  height: 100%;
  border: 1px solid blue;
}

.row {
  width: 100%;
  margin: 10px 0px;
  padding: 10px;
  border: 1px solid purple;
}

.col {
  margin: 0px 10px;
}

.col3 {
  max-width: 270px;
  height: 320px;
}

.col1,
.col2 {
  max-width: 200px;
}
</style>
