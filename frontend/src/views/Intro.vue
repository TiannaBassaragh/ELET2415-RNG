<template>
    <v-container class="container">
        <v-row class="row, bg-surfaceVariant" justify="center">
            <v-col class="col col1" align="center">
                <v-card title="LED A" width="150" density="compact" rounded="md" border flat>
                    <v-divider></v-divider>

                    <v-card-item>
                        <v-icon v-if="payload.ledA == 0" size="50" icon="mdi:mdi-lightbulb"></v-icon>
                        <v-icon v-if="payload.ledA == 1" size="50" icon="mdi:mdi-lightbulb-on" color="yellow" density="compact"></v-icon>
                    </v-card-item>

                    <v-card-item>
                        <v-btn class="text-caption" text="Toggle" variant="tonal" color="primary" @click="toggle('LED A')"></v-btn>
                    </v-card-item>
                </v-card>

                <v-card title="LED B" width="150" density="compact" rounded="md" border flat>
                    <v-divider></v-divider>

                    <v-card-item>
                        <v-icon v-if="payload.ledA == 0" size="50" icon="mdi:mdi-lightbulb"></v-icon>
                        <v-icon v-if="payload.ledA == 1" size="50" icon="mdi:mdi-lightbulb-on" color="yellow" density="compact"></v-icon>
                    </v-card-item>

                    <v-card-item>
                        <v-btn class="text-caption" text="Toggle" variant="tonal" color="primary" @click="toggle('LED B')"></v-btn>
                    </v-card-item>
                </v-card>
            </v-col>

            <v-col class="col2"></v-col>
        </v-row>
        
        <v-row class="row" justify="center">
            <v-col class="bg-black col col3" align="center">
                <span class="digit text-error">
                    <span      v-if="payload.number == 0" >&#x1fbf0;</span>
                    <span v-else-if="payload.number == 1" >&#x1fbf1;</span>
                    <span v-else-if="payload.number == 2" >&#x1fbf2;</span>
                    <span v-else-if="payload.number == 3" >&#x1fbf3;</span>
                    <span v-else-if="payload.number == 4" >&#x1fbf4;</span>
                    <span v-else-if="payload.number == 5" >&#x1fbf5;</span>
                    <span v-else-if="payload.number == 6" >&#x1fbf6;</span>
                    <span v-else-if="payload.number == 7" >&#x1fbf7;</span>
                    <span v-else-if="payload.number == 8" >&#x1fbf8;</span>
                    <span v-else-if="payload.number == 9" >&#x1fbf9;</span>
                </span>
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup>
/** JAVASCRIPT HERE */
const toggle = (name) => {
let message = JSON.stringify({"type":"toggle","device": name}); // Create message and convert to a json string
Mqtt.publish("620162297_sub",message); // Publish message to appropriate topic
}

// IMPORTS
import { ref,reactive,watch ,onMounted,onBeforeUnmount,computed } from "vue";  
import { useRoute ,useRouter } from "vue-router";
import { useMqttStore } from '@/store/mqttStore'; // Import Mqtt Store
import { storeToRefs } from "pinia";
// VARIABLES
const Mqtt = useMqttStore();
const { payload, payloadTopic } = storeToRefs(Mqtt);

const router      = useRouter();
const route       = useRoute();  


// FUNCTIONS
onMounted(()=>{
// THIS FUNCTION IS CALLED AFTER THIS COMPONENT HAS BEEN MOUNTED
Mqtt.connect(); // Connect to Broker located on the backend
setTimeout( ()=>{
// Subscribe to each topic
Mqtt.subscribe("620162297_pub");
Mqtt.subscribe("620162297");
},3000);
});


onBeforeUnmount(()=>{
// THIS FUNCTION IS CALLED RIGHT BEFORE THIS COMPONENT IS UNMOUNTED
// unsubscribe from all topics
Mqtt.unsubcribeAll();
});


</script>


<style scoped>
/** CSS STYLE HERE */
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Symbols+2&display=swap');
.container{
    height:     100%;
    border:     1px solid blue;
}
.row{
    width:      100%;
    padding:    10px;
    margin:     10px 0px;
    border:     1px purple;
}
.col {
    border:     1px solid green;
}
.col1, .col2 {
    max-width:  200px;
}
.col3 {
    height:     320px;
    max-width:  270px;
    border:     2px solid lightslategray;
}
.digit {
    font-family:'Noto Sane Symbols 2';
    font-size:  250px;
}
</style>
  