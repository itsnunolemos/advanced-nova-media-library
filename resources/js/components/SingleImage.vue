<template>
  <gallery-item class="gallery-item-image">
    <div class="gallery-item-info p-3">
      <a v-if="removable" class="delete" href="#" @click.prevent="$emit('remove')">
        <icon type="delete" view-box="0 0 20 20" width="16" height="16" />
      </a>
      <a v-if="isCustomPropertiesEditable" class="edit" href="#" @click.prevent="$emit('editCustomProperties')">
        <icon type="edit" view-box="0 0 20 20" width="16" height="16" />
      </a>
      <a class="preview" :href="image.full_urls.default" target="_blank">
        <icon type="search" view-box="0 0 20 20" width="30" height="30" />
      </a>
    </div>
    <img :src="src" :alt="image.name" class="gallery-image">
  </gallery-item>
</template>

<script>
  import GalleryItem from './GalleryItem';

  export default {
    components: {
      GalleryItem,
    },
    props: ['image', 'field', 'removable', 'isCustomPropertiesEditable'],
    computed: {
      src() {
        // Return desired image conversion on view if it exists
        let conversionOnView = this.field.conversionOnView;

        if (this.image.id && conversionOnView && this.image.full_urls[conversionOnView]) {
          return this.image.full_urls[conversionOnView];
        }
        
        // Return thumnail if conversion exists
        let thumbnail = this.field.thumbnail;

        if (this.image.id && thumbnail && this.image.full_urls[thumbnail]) {
          return this.image.full_urls[thumbnail];
        }

        return this.image.full_urls.default;
      },
    },
  };
</script>

<style lang="scss">
  $bg-color: #e8f5fb;
  $item-max-size: 150px;
  $border-radius: 10px;

  .gallery {
    .gallery-item-image.gallery-item {
      width: $item-max-size;
      height: $item-max-size;

      &:hover .gallery-item-info {
        display: flex;
      }

      .gallery-item-info {
        display: none;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        background-color: transparentize($bg-color, .2);
        border-radius: $border-radius;
        position: absolute;
        z-index: 10;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;

        .preview {
          color: var(--black);
        }

        .delete {
          position: absolute;
          right: 10px;
          top: 10px;
          color: var(--danger);
        }
      }

      .gallery-image {
        object-fit: contain;
        display: block;
        max-height: 100%;
        border-radius: $border-radius;
      }
    }

    .edit {
      position: absolute;
      right: 30px;
      top: 10px;
      color: var(--info);
    }
  }
</style>
