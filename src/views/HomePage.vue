<template>
  <ion-page>

    <ion-header>
      <ion-toolbar>
        <ion-title>My Photo Gallery</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">

      <CameraComponent
        @photo-taken="addPhoto"
      />

      <PhotoGalleryComponent
        :photos="photos"
        @delete-photo="deletePhoto"
      />

    </ion-content>

  </ion-page>
</template>


<script setup lang="ts">

import { ref } from 'vue';

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent
} from '@ionic/vue';

import CameraComponent from '@/components/CameraComponent.vue';
import PhotoGalleryComponent from '@/components/PhotoGalleryComponent.vue';


const photos = ref<string[]>([]);


const addPhoto = (photo: string) => {
  photos.value.unshift(photo);
};


const deletePhoto = (index: number) => {
  photos.value.splice(index, 1);
};

</script>