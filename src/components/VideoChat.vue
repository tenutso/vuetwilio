<script>
import { ref } from "vue";
import Lobby from "@/components/Lobby.vue";
export default {
  components: {
    Lobby
  },
  setup() {
    const username = ref("");
    const roomName = ref("");
    const token = ref("");

    const handleSubmit = async (event) => {
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
          identity: username,
          room: roomName
        })
      }).then((res) => res.json());
      token.value = data.token;
      console.log(data.token);
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
    <p>Token: {{ token }}</p>
  </div>
  <div v-else>
    <Lobby :username="username" :room="roomName" :handleSubmit="handleSubmit" />
  </div>
</template>
