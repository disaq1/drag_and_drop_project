<template>
  <section class="photo-uploader">
    <div class="photo-uploader__progressBar">

    </div>
    <div
        :class="{ 'photo-uploader__wrapper--drag' : isDragStarted }"
        class="photo-uploader__wrapper"
    >
      <input
          @change="uploadFile"
          @dragenter="isDragStarted = true"
          @dragleave="isDragStarted = false"
          ref="input"
          type="file"
          title=""
          multiple
          class="photo-uploader__input"
      >
      {{ uploadText }}
    </div>
    <div class="photo-uploader__photos">
      <div
          v-for="(file, index) in modelValue"
          :key="index"
          class="photo-uploader__photo"
      >
        <button
            @click="onRemove(index)"
            class="photo-uploader__remove"
        />
        <div v-if="file.type !== 'video/mp4' && file.type !== 'video/quicktime'">
          <img
              :src="getUrl(file)"
              :alt="`File № ${index + 1}`"
          >
        </div>
        <div v-if="file.type === 'video/mp4' || 'video/MOV'">
          <video :src="getUrl(file)" autoplay muted />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import {computed, ref, toRefs} from "vue";

export default ({
  props: {
    modelValue: {
      type: Array,
      require: true
    }
  },
  // emits: ['update:modelValue'],
  setup (props, { emit }) {
    const { modelValue } = toRefs(props)

    const input = ref(null)
    const isDragStarted = ref(false)

    const uploadFile = ( {currentTarget} ) => {
      if (currentTarget.files) {
        emit('update:modelValue', [...modelValue.value, ...Array.from(currentTarget.files)])
        console.log(modelValue.value)
      }
      if (input.value) {
        input.value.value = ''
      }
      isDragStarted.value = false
    }

    const onRemove = (index) => {
      emit('update:modelValue', modelValue.value.filter((p, i) => i !== index))
    }

    const uploadText = computed(() => {
      if (needToUpload.value > 0) {
        return `Загрузите ещё ${needToUpload.value} ${needToUpload.value === 1 ? 'ваш файл' : 'ваших файла'}`
      }
      if (needToUpload.value === 0) {
        return 'Вы загрузили все файлы'
      }
      return 'Вы загрузили слишком много файлов'
    })

    const getUrl = (photo) => URL.createObjectURL(photo)

    const needToUpload = computed(() => 4 - modelValue.value.length)

    return {
      isDragStarted,
      input,
      uploadFile,
      onRemove,
      needToUpload,
      uploadText,
      getUrl
    }
  }
});
</script>

<style lang="scss">
  .photo-uploader {
    &__url {
      width: 100%;
      margin: 0 0 20px;
      &-wrapper {
        display: flex;
        & span {
          width: 20%;
          margin: 0 10px 0 0;
        }
      }
    }
    &__wrapper {
      position: relative;
      text-align: center;
      height: 150px;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 4px dotted grey;
      color: rgba(0, 0, 0, .5);
      &--drag {
        border: 4px dotted black;
      }
    }
    &__input {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      z-index: 2;
      opacity: 0;
      cursor: pointer;
    }
    &__submit {
      position: absolute;
      width: 150px;
      height: 40px;
      bottom: -50px;
      right: 0;
      cursor: pointer;
    }
    &__photos {
      display: flex;
      margin-top: 20px;
    }
    &__photo {
      position: relative;
      width: 200px;
      margin-right: 60px;
      & img,
      & video{
        width: 200px;
        border-radius: 10px;
      }
    }
    &__remove {
      cursor: pointer;
      position: absolute;
      outline: none;
      top: 0;
      left: calc(100% + 10px);
      width: 30px;
      height: 30px;
      border-radius: 50%;
      background: rgba(0, 0, 0, .05);
      border: 0;
      &:before {
        content: '';
        position: absolute;
        z-index: 1;
        top: calc(50% - 1px);
        left: calc(50% - 5px);
        width: 10px;
        height: 2px;
        background: black;
      }
    }
  }
</style>
