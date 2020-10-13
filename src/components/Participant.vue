<script>
import { reactive, ref, onMounted, onUnmounted } from "vue";

export default {
  props: {
    participant: Object
  },
  setup(props) {
    const state = reactive({
      videoTracks: [],
      audioTracks: []
    });
    const videoRef = ref();
    const audioRef = ref();

    const trackpubsToTracks = trackMap =>
      Array.from(trackMap.values())
        .map(publication => publication.track)
        .filter(track => track !== null);

    function trackSubscribed(track) {
      if (track.kind === "video") {
        state.videoTracks.push(track);
      } else {
        state.audioTracks.push(track);
      }
    }
    function trackUnsubscribed(track) {
      if (track.kind === "video") {
        state.videoTracks.filter(v => v !== track);
      } else {
        state.audioTracks.filter(v => v !== track);
      }
    }

    onMounted(() => {
      const videoTrack = state.videoTracks[0];
      if (videoTrack) {
        state.videoTrack.attach(videoRef.value);
      }
      const audioTrack = state.audioTracks[0];
      if (audioTrack) {
        state.audioTrack.attach(audioRef.value);
      }
    });

    onUnmounted(() => {
      state.videoTrack.detach();
      state.audioTrack.detach();
    });

    state.videoTracks.push(trackpubsToTracks(props.participant.videoTracks));
    state.audioTracks.push(trackpubsToTracks(props.participant.audioTracks));

    props.participant.on("trackSubscribed", trackSubscribed);
    props.participant.on("trackUnsubscribed", trackUnsubscribed);

    return { state, videoRef, audioRef };
  }
};
</script>
<template>
  <div class="participant">
    <h3>{{ participant.identity }}</h3>
    <video :ref="videoRef" :autoPlay="true" />
    <audio :ref="audioRef" :autoPlay="true" :muted="true" />
  </div>
</template>
