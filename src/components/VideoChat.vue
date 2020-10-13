<script>
import { ref } from "vue";
import Lobby from "@/components/Lobby.vue";
import Room from "@/components/Room.vue";
export default {
  components: {
    Lobby,
    Room
  },
  setup() {
    const username = ref("");
    const roomName = ref("");
    const token = ref("");

    const handleSubmit = async event => {
      event.preventDefault();
      const form = new FormData(event.target);
      username.value = form.get("username");
      roomName.value = form.get("room");

      const data = await fetch("/get_video_token", {
        headers: {
          "Content-Type": "application/json"
        },
        method: "POST",
        body: JSON.stringify({
          identity: username.value,
          room: roomName.value
        })
      }).then(res => res.json());
      token.value = data.token;
    };
    const handleLogout = () => {
      token.value = null;
    };
    return { username, roomName, token, handleSubmit, handleLogout };
  }
};
</script>
<template>
  <div v-if="token">
    <p>Username: {{ username }}</p>
    <p>Room name: {{ roomName }}</p>
    <!--<p>Token: {{ token }}</p>-->
    <Room :roomName="roomName" :token="token" :handleLogout="handleLogout" />
  </div>
  <div v-else>
    <Lobby :username="username" :room="roomName" :handleSubmit="handleSubmit" />
  </div>
</template>
