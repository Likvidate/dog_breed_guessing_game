
<template>
  <transition name="modal-fade">
    <div class="modal-backdrop">
      <div class="modal"
        role="dialog"
        aria-labelledby="modalTitle"
        aria-describedby="modalDescription"
      >
        <section
          class="modal-body"
          id="modalDescription"
        >
          <slot name="body">
              <div><h3>Your Score was: {{score}}</h3></div>
              <div><h3>Correct Answer was: {{correctAnswer}}</h3></div>
          </slot>
        </section>

        <footer class="modal-footer">
          <button
            type="button"
            class="btn-red"
            @click="close"
            aria-label="Close modal"
          >
            Restart
          </button>
        </footer>
      </div>
    </div>
  </transition>
</template>

<script>
  export default {
    name: 'Modal',
    props: {
        score: Number,
        correctAnswer: String
    },
    methods: {
      close() {
        this.$emit('close');
      },
    },
  };
</script>

<style>
h3 {
    color: aqua;
}
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: black;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }
  .modal-footer {
    padding: 15px;
    display: flex;
  }
  .modal-footer {
    border-top: 1px solid #eeeeee;
    flex-direction: column;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-red {
    color: white;
    background: black;
    border: 1px solid aqua;
    border-radius: 2px;
    height: 50px;
  }
  .btn-red:hover {
    cursor: pointer;
    color: white;
    border: 1px solid aqua;
    background-color: aqua;
  }

  .modal-fade-enter,
  .modal-fade-leave-to {
    opacity: 0;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity .5s ease;
  }
</style>