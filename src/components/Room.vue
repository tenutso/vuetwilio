<script>
/* eslint-disable */
import Video from "twilio-video";
import { onMounted, onUnmounted, reactive, toRefs, toRef } from "vue";
import Participant from "@/components/Participant.vue";
export default {
  components: {
    Participant
  },
  props: {
    roomName: String,
    token: String,
    handleLogout: Function
  },
  /* eslint-enable */
  setup(props) {
    const data = reactive({
      participants: [],
      room: null
    });

    let { room, participants } = toRefs(data);

    onMounted(async () => {
      const participantConnected = participant => {
        console.log(`Participant ${participant.identity} connected'`);
        data.participants.push(participant);
      };

      const participantDisconnected = participant => {
        console.log(`Participant ${participant.identity} disconnected.`);
        data.participants = data.participants.filter(p => p !== participant);
      };

      data.room = await Video.connect(props.token, {
        name: props.roomName
      });

      console.log(`Connected to Room ${data.room}`);
      data.room.participants.forEach(participantConnected);
      data.room.on("participantConnected", participantConnected);
      data.room.on("participantDisconnected", participantDisconnected);

      console.log("ROOMDATA", data.room);
    });

    onUnmounted(() => {
      if (data.room.localParticipant.state === "connected") {
        data.room.localParticipant.tracks.forEach(function(trackPublication) {
          trackPublication.track.stop();
        });
        data.room.disconnect();
      }
    });

    return { room, participants };
  }
};
</script>
<template>
  <div class="room">
    <h2>Room: {{ roomName }}</h2>

    <button @click="handleLogout">Log out</button>
    <div class="local-participant">
      <h3>Local Participant</h3>
      <div v-if="room">
        REACTIVE!!!
        <p :key="room.localParticipant.sid">
          Name{{ room.localParticipant.identity }}
        </p>
      </div>
    </div>
    <h3>Remote Participants</h3>
    <div
      v-for="participant in participants"
      :key="participant.sid"
      class="remote-participants"
    >
      <p :key="participant.sid">{{ participant.identity }}</p>
    </div>
  </div>
</template>
