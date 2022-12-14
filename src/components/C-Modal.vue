<template>
  <transition name="fade">
    <div class="popup" v-if="show">
      <div class="popup__backdrop" @click="closeModal()" />
      <div class="popup__dialog">
        <div class="popup__header">
          <span>Добавить столбец</span>
          <img src="./../assets/close.png" alt="close" @click="closeModal">
        </div>

        <div class="popup__body">
          <input type="text" name="row-heading" id="row-heading" placeholder="Название столбца" v-model="col.heading">
          <div style="display: flex; justify-content: space-between; margin-top: 20px">
            <label for="row-order" style="margin-right: 20px">Порядок столбца:</label>
            <input style="max-width: 50px" type="number" name="row-order" id="row-order" v-model="col.order">
          </div>
        </div>
        <div class="popup__footer">
          <button @click="saveText">Сохранить</button>
          <button @click="closeModal">Закрыть</button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'C-Modal',
  data() {
    return {
      show: false,
      col: {
        heading: '',
        order: 1
      }
    }
  },
  methods: {
    closeModal() {
      this.show = false;
      this.$emit('close-modal')
      document.querySelector('body').classList.remove('overflow-hidden');
    },
    openModal() {
      this.show = true;
      document.querySelector('body').classList.add('overflow-hidden');
    },
    saveText() {
      if (this.col.order <= 0) {
        this.col.order = 1
      }
      if (this.col.heading.trim()) {
        this.$emit('save-card', this.col)
        this.col = {
          heading: '',
          order: 1
        }
        this.closeModal()
      }
    },
  }
};
</script>

<style lang="scss" scoped>
.popup {
  overflow-x: hidden;
  overflow-y: auto;
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 9999;
  &__backdrop {
    background-color: rgba(0, 0, 0, 0.3);
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 1111;
  }
  &__dialog {
    background-color: #ffffff;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: fit-content;
    min-width: 400px;
    max-width: 1200px;
    display: flex;
    flex-direction: column;
    border-radius: 5px;
    z-index: 1112;
    @media screen and (max-width: 992px) {
      width: 90%;
    }
  }
  &__header {
    padding: 20px 20px 10px;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    img {
      width: 20px;
      height: 20px;
      cursor: pointer;
      position: absolute;
      right: 20px;
      top: 20px;
    }
  }
  &__body {
    padding: 10px 20px 10px;
    overflow: auto;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    text-align: center;
  }
  &__footer {
    padding: 10px 20px 20px;
  }
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>