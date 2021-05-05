<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Square Video</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Square Video</ion-title>
        </ion-toolbar>
      </ion-header>
      <VideoPlayer />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import VideoPlayer from '@/components/VideoPlayer.vue';

export default  {
  name: 'SquareVideo',
  components: { VideoPlayer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage }
}
</script>