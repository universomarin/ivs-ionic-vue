<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Live Stream</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Live Stream</ion-title>
        </ion-toolbar>
      </ion-header>
      <VideoPlayer />
    </ion-content>
  </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import VideoPlayer from '@/components/VideoPlayer.vue';

export default  {
  name: 'LiveStream',
  props: {
    autoplay: Boolean
  },
  components: { VideoPlayer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage }
}
</script>