<script>
//import { computed } from "vue";
export default {
  props: {
    username: String,
    roomName: String,
    handleSubmit: Function,
    handleLogout: Function
  },
  setup() {
    //console.log(props);
  }
};
</script>
<template>
  <form @submit="handleSubmit">
    <h2>Enter a room</h2>
    <div>
      <label htmlFor="name">Name:</label>
      <input type="text" id="name" name="username" :value="username" required />
    </div>

    <div>
      <label htmlFor="room">Room name:</label>
      <input type="text" id="room" name="room" :value="roomName" required />
    </div>
    <button type="submit">Submit</button>
  </form>
</template>
