<template>
  <ion-card>

    <ion-card-header>
      <ion-card-title>Photo Gallery</ion-card-title>
    </ion-card-header>

    <ion-card-content>

      <!-- Kapag walang picture -->
      <div v-if="photos.length === 0" class="empty-gallery">
        <ion-icon :icon="imagesIcon"></ion-icon>
        <p>No photos yet.</p>
      </div>

      <!-- Kapag may pictures -->
      <ion-grid v-else>
        <ion-row>

          <ion-col
            v-for="(photo, index) in photos"
            :key="index"
            size="6"
            size-md="4"
          >

            <ion-card class="photo-card">

              <ion-img
                :src="photo"
                class="gallery-image"
              ></ion-img>

              <ion-card-content class="photo-actions">

                <ion-button
                  expand="block"
                  color="danger"
                  size="small"
                  @click="deletePhoto(index)"
                >
                  Delete
                </ion-button>

              </ion-card-content>

            </ion-card>

          </ion-col>

        </ion-row>
      </ion-grid>

    </ion-card-content>

  </ion-card>
</template>


<script setup lang="ts">

import {
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
  IonButton,
  IonIcon
} from '@ionic/vue';

import { images as imagesIcon } from 'ionicons/icons';


const props = defineProps<{
  photos: string[];
}>();


const emit = defineEmits<{
  (event: 'delete-photo', index: number): void;
}>();


const deletePhoto = (index: number) => {
  emit('delete-photo', index);
};

</script>


<style scoped>

.empty-gallery {
  text-align: center;
  padding: 40px 20px;
}

.empty-gallery ion-icon {
  font-size: 55px;
  color: #999;
}

.empty-gallery p {
  margin-top: 10px;
  color: #777;
  font-size: 15px;
}

.photo-card {
  margin: 5px;
  border-radius: 10px;
  overflow: hidden;
}

.gallery-image {
  width: 100%;
  height: 180px;
  object-fit: cover;
}

.photo-actions {
  padding: 8px;
}

</style>