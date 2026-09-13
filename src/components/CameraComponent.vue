```vue
<template>
  <ion-card class="camera-card">

    <ion-card-content>

      <!-- Camera Header -->
      <div class="camera-header">
        <div class="camera-icon-wrapper">
          <ion-icon :icon="cameraIcon"></ion-icon>
        </div>

        <div>
          <h2>Take a Picture</h2>
          <p>Capture an image using your camera</p>
        </div>
      </div>

      <!-- Camera Preview Area -->
      <div class="camera-preview">
        <ion-icon :icon="cameraIcon" class="preview-icon"></ion-icon>

        <h3>Ready to capture</h3>
        <p>
          Tap the button below to open your camera
          and take a picture.
        </p>
      </div>

      <!-- Error Message -->
      <ion-text v-if="errorMessage" color="danger">
        <div class="error-message">
          {{ errorMessage }}
        </div>
      </ion-text>

      <!-- Take Picture Button -->
      <ion-button
        expand="block"
        class="take-picture-btn"
        @click="takePicture"
      >
        <ion-icon slot="start" :icon="cameraIcon"></ion-icon>
        Take Picture
      </ion-button>

    </ion-card-content>

  </ion-card>
</template>

<script setup lang="ts">

import {
  IonCard,
  IonCardContent,
  IonButton,
  IonIcon,
  IonText
} from '@ionic/vue';

import { camera as cameraIcon } from 'ionicons/icons';

import {
  Camera,
  CameraResultType,
  CameraSource
} from '@capacitor/camera';

import { ref } from 'vue';

const errorMessage = ref('');

/*
 * Ipapadala nito ang captured photo
 * papunta sa parent component (HomePage.vue)
 */
const emit = defineEmits<{
  (event: 'photo-taken', photo: string): void;
}>();

const takePicture = async () => {

  try {

    errorMessage.value = '';

    const image = await Camera.getPhoto({
      quality: 90,
      allowEditing: false,
      resultType: CameraResultType.DataUrl,
      source: CameraSource.Camera
    });

    console.log('Photo taken:', image.dataUrl);

    // I-send ang photo sa HomePage.vue
    if (image.dataUrl) {
      emit('photo-taken', image.dataUrl);
    }

  } catch (error) {

    console.error(error);

    errorMessage.value = 'Unable to take picture. Please try again.';

  }

};

</script>

<style scoped>

.camera-card {
  margin: 16px;
  border-radius: 20px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
}

/* Header */

.camera-header {
  display: flex;
  align-items: center;
  gap: 14px;
  margin-bottom: 20px;
}

.camera-icon-wrapper {
  width: 52px;
  height: 52px;
  border-radius: 16px;

  display: flex;
  align-items: center;
  justify-content: center;

  background: #e8f1ff;
}

.camera-icon-wrapper ion-icon {
  font-size: 28px;
  color: #2563eb;
}

.camera-header h2 {
  margin: 0;
  font-size: 20px;
  font-weight: 700;
  color: #1e293b;
}

.camera-header p {
  margin: 4px 0 0;
  font-size: 13px;
  color: #64748b;
}

/* Preview */

.camera-preview {
  min-height: 220px;

  border: 2px dashed #cbd5e1;
  border-radius: 18px;

  background: #f8fafc;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  text-align: center;

  padding: 25px;
  margin-bottom: 18px;
}

.preview-icon {
  font-size: 55px;
  color: #2563eb;
  margin-bottom: 10px;
}

.camera-preview h3 {
  margin: 5px 0;
  font-size: 17px;
  font-weight: 600;
  color: #334155;
}

.camera-preview p {
  max-width: 270px;
  margin: 5px auto 0;

  font-size: 13px;
  line-height: 1.5;

  color: #64748b;
}

/* Button */

.take-picture-btn {
  --background: #2563eb;
  --background-hover: #1d4ed8;

  --border-radius: 14px;

  height: 50px;

  font-size: 15px;
  font-weight: 600;

  margin-top: 5px;
}

/* Error */

.error-message {
  background: #fef2f2;
  border-radius: 10px;

  padding: 10px 12px;
  margin-bottom: 12px;

  font-size: 13px;
  text-align: center;
}

</style>
