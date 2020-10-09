<script>
import { ref } from "vue";
import VideoChat from "@/components/VideoChat.vue";
export default {
  components: {
    VideoChat
  },
  setup() {
    /* eslint-disable */
    const username = ref("");
    const roomName = ref("");
    const token = ref("");
    /* eslint-enable */
  }
};
</script>
<template>
  <div className="app">
    <header>
      <h1>Video Chat with Hooks</h1>
    </header>
    <main>
      <VideoChat />
    </main>
    <footer>
      <p>
        Devel: Anthony Nijmeh
      </p>
    </footer>
  </div>
</template>
