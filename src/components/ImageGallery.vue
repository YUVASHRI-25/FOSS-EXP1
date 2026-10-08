<template>
  <div class="gallery-container">
    <h1 class="gallery-title">🍦 Ice Cream Gallery</h1>

    <div v-if="deletedImages.length > 0" class="restore-section">
      <button @click="restoreLastDeleted" class="restore-button">
        Restore Last Deleted
      </button>
    </div>

    <div v-if="images.length === 0" class="no-images">
      No images available.
    </div>

    <div v-else class="gallery-grid">
      <div
        v-for="(image, index) in images"
        :key="image.id"
        class="image-card"
        @click="openLightbox(index)"
      >
        <button
          class="delete-button"
          @click.stop="deleteImage(index)"
        >
          Delete
        </button>
        <img
          :src="image.src"
          :alt="image.title"
          class="gallery-image"
        />
      </div>
    </div>

    <!-- Lightbox Modal -->
    <div
      v-if="lightboxOpen"
      class="lightbox"
      @click.self="closeLightbox"
    >
      <div class="lightbox-content">
        <button class="close-button" @click="closeLightbox">×</button>
        <button
          class="nav-button prev-button"
          @click="prevImage"
          :disabled="currentIndex === 0"
        >
          ‹
        </button>
        <button
          class="nav-button next-button"
          @click="nextImage"
          :disabled="currentIndex === images.length - 1"
        >
          ›
        </button>

        <div class="image-viewer" @wheel.prevent="handleWheel">
          <img
            :src="currentImage.src"
            :alt="currentImage.title"
            class="lightbox-image"
            :style="{ transform: `scale(${zoomLevel})` }"
          />
        </div>

        <div class="image-details">
          <input
            v-model="currentImage.title"
            class="image-title-input"
            placeholder="Image title"
          />
          <textarea
            v-model="currentImage.description"
            class="image-description"
            placeholder="Image description"
            rows="3"
          ></textarea>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImageGallery',
  data() {
    return {
      images: [],
      deletedImages: [],
      lightboxOpen: false,
      currentIndex: 0,
      zoomLevel: 1
    };
  },
  computed: {
    currentImage() {
      if (this.images.length === 0) {
        return { src: '', title: '', description: '' };
      }
      return this.images[this.currentIndex];
    }
  },
  created() {
    this.initializeImages();
  },
  methods: {
    initializeImages() {
      const imageNames = [
        'Image1.jpeg',
        'Image2.jpeg',
        'Image3.jpeg',
        'Image4.jpeg',
        'Image5.jpeg',
        'Image6.jpeg',
        'Image7.jpeg',
        'Image8.jpeg',
        'Image9.jpeg',
        'Image10.jpeg',
        'Image11.jpeg',
        'Image12.jpeg',
        'Image13.jpeg',
        'Image14.jpeg',
        'Image15.jpeg'
      ];

      this.images = imageNames.map((name, index) => ({
        id: index + 1,
        src: `/images/${name}`,
        title: `Ice Cream ${index + 1}`,
        description: ''
      }));
    },
    deleteImage(index) {
      const deletedImage = this.images.splice(index, 1)[0];
      this.deletedImages.push(deletedImage);

      // If the deleted image was the one shown in lightbox
      if (this.lightboxOpen && index === this.currentIndex) {
        this.closeLightbox();
      }
      // If we deleted an image before the current index, adjust the index
      else if (this.lightboxOpen && index < this.currentIndex) {
        this.currentIndex--;
      }
    },
    restoreLastDeleted() {
      if (this.deletedImages.length === 0) return;

      const lastDeleted = this.deletedImages.pop();
      
      // Find the appropriate position to restore
      let insertIndex = 0;
      for (let i = 0; i < this.images.length; i++) {
        if (this.images[i].id > lastDeleted.id) {
          insertIndex = i;
          break;
        }
        insertIndex = i + 1;
      }

      this.images.splice(insertIndex, 0, lastDeleted);

      // Adjust current index if needed
      if (this.lightboxOpen && insertIndex <= this.currentIndex) {
        this.currentIndex++;
      }
    },
    openLightbox(index) {
      this.currentIndex = index;
      this.zoomLevel = 1;
      this.lightboxOpen = true;
    },
    closeLightbox() {
      this.lightboxOpen = false;
      this.zoomLevel = 1;
    },
    prevImage() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
        this.zoomLevel = 1;
      }
    },
    nextImage() {
      if (this.currentIndex < this.images.length - 1) {
        this.currentIndex++;
        this.zoomLevel = 1;
      }
    },
    handleWheel(event) {
      if (event.deltaY < 0) {
        // Wheel up - zoom in
        this.zoomLevel = Math.min(this.zoomLevel + 0.1, 2);
      } else {
        // Wheel down - zoom out
        this.zoomLevel = Math.max(this.zoomLevel - 0.1, 0.5);
      }
    }
  }
};
</script>

<style scoped>
.gallery-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 40px 20px;
}

.gallery-title {
  text-align: center;
  font-size: 2.5rem;
  color: #333;
  margin-bottom: 30px;
  font-weight: 600;
}

.restore-section {
  text-align: center;
  margin-bottom: 20px;
}

.restore-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.restore-button:hover {
  background-color: #45a049;
}

.no-images {
  text-align: center;
  font-size: 1.5rem;
  color: #666;
  margin-top: 50px;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 20px;
  margin-top: 20px;
}

.image-card {
  position: relative;
  width: 100%;
  padding-bottom: 100%;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s, box-shadow 0.3s;
}

.image-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.delete-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #ff4444;
  color: white;
  border: none;
  padding: 5px 12px;
  font-size: 12px;
  border-radius: 5px;
  cursor: pointer;
  z-index: 10;
  transition: background-color 0.3s;
}

.delete-button:hover {
  background-color: #cc0000;
}

.gallery-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* Lightbox Styles */
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.lightbox-content {
  position: relative;
  max-width: 90%;
  max-height: 90%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.close-button {
  position: absolute;
  top: -40px;
  right: 0;
  background-color: transparent;
  color: white;
  border: none;
  font-size: 40px;
  cursor: pointer;
  z-index: 1001;
  transition: color 0.3s;
}

.close-button:hover {
  color: #ff4444;
}

.nav-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(255, 255, 255, 0.3);
  color: white;
  border: none;
  font-size: 50px;
  padding: 20px 30px;
  cursor: pointer;
  z-index: 1001;
  transition: background-color 0.3s;
}

.nav-button:hover:not(:disabled) {
  background-color: rgba(255, 255, 255, 0.5);
}

.nav-button:disabled {
  opacity: 0.3;
  cursor: not-allowed;
}

.prev-button {
  left: -80px;
}

.next-button {
  right: -80px;
}

.image-viewer {
  max-width: 80vw;
  max-height: 60vh;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}

.lightbox-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  transition: transform 0.1s ease-out;
}

.image-details {
  margin-top: 20px;
  width: 100%;
  max-width: 600px;
}

.image-title-input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: rgba(255, 255, 255, 0.9);
}

.image-description {
  width: 100%;
  padding: 10px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 5px;
  resize: vertical;
  background-color: rgba(255, 255, 255, 0.9);
}

/* Responsive Design */
@media (max-width: 1024px) {
  .gallery-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .gallery-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .nav-button {
    font-size: 30px;
    padding: 10px 20px;
  }

  .prev-button {
    left: -50px;
  }

  .next-button {
    right: -50px;
  }
}

@media (max-width: 480px) {
  .gallery-title {
    font-size: 1.8rem;
  }

  .gallery-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .image-viewer {
    max-width: 95vw;
    max-height: 50vh;
  }
}
</style>
