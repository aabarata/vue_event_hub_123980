<template>
  <transition name="app-modal">
    <div class="modal-wrapper" v-if="isOpen" @click.self="close">
      <div class="modal">
        <component
          :is="content"
          :data="data"
          @submit="processSubmit"
          @cancel="processCancel"
        ></component>
      </div>
    </div>
  </transition>
</template>

<script>
import Content1 from "@/components/Content1.vue";
import Content2 from "@/components/Content2.vue";
import Content3 from "@/components/Content3.vue";

export default {
  name: "Modal",
  components: { Content1, Content2, Content3 },
  data() {
    return {
      isOpen: false,
      content: "content1",
      data: {},
    };
  },
  created: function () {
    this.$eventHub.$on("open-modal", this.open);
    this.$eventHub.$on("close-modal", this.close);
  },
  beforeDestroy: function () {
    this.$eventHub.$off("open-modal", this.open);
    this.$eventHub.$off("close-modal", this.close);
  },
  methods: {
    open(payload) {
      this.isOpen = true;
      this.content = payload.content;
      this.data = payload.data;
    },
    close() {
      this.isOpen = false;
    },
    processSubmit(data) {
      this.$eventHub.$emit("validate-modal", data);
      this.isOpen = false;
    },
    processCancel() {
      this.isOpen = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.modal-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.4);
  .modal {
    width: 50%;
    height: 50%;
    background: white;
    color: black;
  }
}
.app-modal-enter-active {
  transition: all 0.3s ease;
  .modal {
    transition: all 0.3s ease;
  }
}
.app-modal-leave-active {
  transition: all 0.6s cubic-bezier(1, 0.5, 0.8, 1);
  .modal {
    transition: all 0.6s cubic-bezier(1, 0.5, 0.8, 1);
  }
}
.app-modal-enter,
.app-modal-leave-to {
  opacity: 0;
}
.app-modal-enter,
.app-modal-leave-to {
  .modal {
    transform: scale(1.1);
  }
}
</style>
