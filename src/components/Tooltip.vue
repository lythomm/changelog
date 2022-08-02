<template>
  <div class="container">
    <fa v-if="!changelog.added || !changelog.changed || !changelog.fixed || !changelog.removed" icon="circle-plus" :style="{ color: '#5968E5', cursor: 'pointer' }" size="2x"  @click="dropdown = !dropdown" />
    <Transition name="slide-fade">
      <div class="dropdown" v-if="dropdown">
        <div class="section added" v-if="!changelog.added">
          <span @click="createSection('added')">AJOUTÉ</span>
        </div>
        <div class="section changed" v-if="!changelog.changed">
          <span @click="createSection('changed')">CHANGÉ</span>
        </div>
        <div class="section fixed" v-if="!changelog.fixed">
          <span @click="createSection('fixed')">CORRIGÉ</span>
        </div>
        <div class="section removed" v-if="!changelog.removed">
          <span @click="createSection('removed')">SUPPRIMÉ</span>
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
      dropdown: true,
      changelogEmpty: false
    }
  },
  methods: {
    createSection (type) {
      this.$emit('new-section', this.changelogId, type)
      this.disableTooltip()
    },
    // Automatically close tooltip if not needed
    disableTooltip () {
      if (this.changelog.added && this.changelog.changed && this.changelog.fixed && this.changelog.removed ) {
        this.dropdown = false
      }
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  position: relative;
  padding-bottom: 1rem;
}

.dropdown {
  position: absolute;
  top: -4.5rem;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  background: white;
  border-radius: 8px;
  height: 40px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
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
  margin: 0 0.25rem;
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
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(20px);
  opacity: 0;
}
</style>