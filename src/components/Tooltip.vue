<template>
  <div class="container">
    <fa icon="circle-plus" :style="{ color: '#5968E5', cursor: 'pointer' }" size="2x"  @click="dropdown = !dropdown" />
    <Transition name="slide-fade">
      <div class="dropdown" v-if="dropdown">
        <div class="section added" style="margin:0 10px" v-if="!changelog.added">
          <span>AJOUTÉ</span>
        </div>
        <div class="section changed" style="margin-left:10px" v-if="!changelog.changed">
          <span>CHANGÉ</span>
        </div>
        <div class="section fixed" style="margin-left:10px" v-if="!changelog.fixed">
          <span>CORRIGÉ</span>
        </div>
        <div class="section removed" style="margin:0 10px" v-if="!changelog.removed">
          <span>SUPPRIMÉ</span>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script>
export default {
  name: 'Tool-tip',
  props: {
    changelogId: Number,
    type: String,
    changelog: Object
  },
  data () {
    return {
      dropdown: true
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  position: relative;
}

.dropdown {
  position: absolute;
  bottom: -5rem;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  background: white;
  border-radius: 8px;
  height: 40px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}

.dropdown::before {
  /* --translate-y: calc(-1 * 10px); */
  transform: rotate(180deg);
  position: absolute;
  left: 50%;
  top: -19px;
  content: '';
  border: 10px solid transparent;
  border-top-color: red;
}

.section {
  border-radius: 8px;
  color: white;
  font-weight: 1000;
  line-height: 10px;
  letter-spacing: 2px;
  height: 30px;
  display: flex;
  align-items: center;
  padding: 0 1rem;
  cursor: pointer;
}

.added {
  background: #4CAF50;
}

.changed {
  background: #FF9800;
}

.fixed {
  background: #2196F3;
}

.removed {
  background: #f44336;
}

/* Animations & Transitions */
.slide-fade-enter-active {
  transition: all 0.5s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>