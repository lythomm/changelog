<template>
  <div class="container" v-on:keyup.enter="SaveToLocalStorage">

    <h1>Changelog v1.0.0</h1>
  
    <div>
      <!-- <button class="topBtn" @click="SaveToLocalStorage" style="margin-right: 1rem">Sauvegarder</button> -->
      <!-- <button class="topBtn" @click="resetSave" style="margin-right: 1rem">RESET</button> -->
      <!-- <button class="topBtn" v-if="changelogActive" @click="deleteChangelog" style="margin-left: 1rem">Supprimer le changelog</button> -->
    </div>


    <div v-if="!changelogActive">
      <div style="margin-top: 1rem">
        <fa icon="circle-plus" :style="{ color: '#5968E5', cursor: 'pointer' }" size="2x" @click="createNewChangelog" />
      </div>
    </div>

    <div v-else>
      <div v-for="(changelog, changelogIndex) in changelogs" :key="changelog.id">
        <div class="addNewSection" />
        <div class="hide">
          <fa icon="circle-plus" :style="{ color: '#5968E5', cursor: 'pointer' }" size="2x" @click="createNewChangelog(changelogIndex)" />
        </div>
        <div style="display: flex">
          <div class="changelog__section-body">
            <div style="display: flex; align-items: center">
              <input class="version" style="color: #5968E5" v-model="changelog.ver" />
              <div class="separator" />
              <input class="date" style="font-weight: bold" v-model="changelog.date" />
            </div>
            <!-- <Tooltip :changelogId="changelogIndex" :type="'added'" :changelog="changelog" @new-section="createNewSection" /> -->
            <div class="changelog__section-container">
              <div v-if="changelog.added">
                <div style="display: flex; align-items: center">
                  <button class="changelog__section-container-btn add">AJOUTÉ</button>
                  <CloseCircle @click="deleteSection('added', changelogIndex)" :size="30" fillColor="red" class="delete" />
                </div>
                <ul v-for="(add, index) in changelog.added" :key="index">
                  <li>
                    <div style="display: flex">
                      <input class="text" v-model="changelog.added[index]" @keyup.enter="newLine(changelogIndex, 'added', index)" />
                      <CloseCircle @click="deleteLine(changelogIndex, 'added', index)" :size="15" fillColor="red" class="delete" />
                    </div>
                  </li>
                </ul>
              </div>
            </div>
            <div class="changelog__section-container">
              <div v-if="changelog.changed">
                <div style="display: flex; align-items: center">
                  <button class="changelog__section-container-btn changed">CHANGÉ</button>
                  <CloseCircle @click="deleteSection('changed', changelogIndex)" :size="30" fillColor="red" class="delete" />
                </div>
                <ul v-for="(change, index) in changelog.changed" :key="index">
                  <li class="editable" contenteditable="true" @blur="SaveToLocalStorage">
                    <div style="display: flex">
                      <input class="text" v-model="changelog.changed[index]" @keyup.enter="newLine(changelogIndex, 'changed', index)" />
                      <CloseCircle @click="deleteLine(changelogIndex, 'changed', index)" :size="15" fillColor="red" class="delete" />
                    </div>
                  </li>
                </ul>
              </div>
            </div>
            <div class="changelog__section-container">
              <div v-if="changelog.fixed">
                <div style="display: flex; align-items: center">
                  <button class="changelog__section-container-btn fixed">CORRIGÉ</button>
                  <CloseCircle @click="deleteSection('fixed', changelogIndex)" :size="30" fillColor="red" class="delete" />
                </div>
                <ul v-for="(fix, index) in changelog.fixed" :key="index">
                  <li>
                    <div style="display: flex">
                      <input class="text" v-model="changelog.fixed[index]" @keyup.enter="newLine(changelogIndex, 'fixed', index)" />
                      <CloseCircle @click="deleteLine(changelogIndex, 'fixed', index)" :size="15" fillColor="red" class="delete" />
                    </div>
                  </li>
                </ul>
              </div>
            </div>
            <div class="changelog__section-container">
              <div v-if="changelog.removed">
                <div style="display: flex; align-items: center">
                  <button class="changelog__section-container-btn removed">SUPPRIMÉ</button>
                  <CloseCircle @click="deleteSection('removed', changelogIndex)" :size="30" fillColor="red" class="delete" />
                </div>
                <ul v-for="(remove, index) in changelog.removed" :key="index">
                  <li>
                    <div style="display: flex">
                      <input class="text" v-model="changelog.removed[index]" @keyup.enter="newLine(changelogIndex, 'removed', index)" />
                      <CloseCircle @click="deleteLine(changelogIndex, 'removed', index)" :size="15" fillColor="red" class="delete" />
                    </div>
                  </li>
                </ul>
              </div>
              <Tooltip :changelogId="changelogIndex" :type="'added'" :changelog="changelog" @new-section="createNewSection" />
            </div>
          </div>
          <div style="display: flex; align-items: center; margin-right: -3.5rem; margin-left: 1rem">
            <div class="deleteCurrentChangelog" />
            <CloseCircle @click="deletelogCurrentChangelog(changelogIndex)" :size="30" fillColor="red" class="hideDeleteChangelog" />
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import CloseCircle from 'vue-material-design-icons/CloseCircle.vue'
import Tooltip from './Tooltip.vue'

export default {
  name: 'Change-log',
  components: {
    CloseCircle,
    Tooltip
  },
  data () {
    return {
      changelogActive: false,
      changelogs: [],
      unchangedChangelogs: [
        {
          ver: 'v2.46.3',
          date: '05/07/2022',
          added: [
            'Le champ “email” est désormais obligatoire lors de la création d’un patient',
            'En version dégradé, ajout d’un bouton permettant de revenir en mode Normale'
          ],
          changed: [
            'La popup de choix de contenu met désormais un temps raisonnable à disparaître'
          ],
          fixed: [
            'Correction d’un bug qui empêchait de lancer un exercice en mode hors-dossier',
            'Correction d’un bug où l’exportation des textes de contenus ne fonctionnait pas'
          ],
          removed: [
            'Suppression de l\'ancienne méthode d\'exportation de textes.'
          ]
        },
        {
          ver: 'v2.46.0',
          date: '04/07/2022',
          added: [
            'Message d’erreur si le contenu ne se télécharge pas côté particulier',
          ],
          changed: [
            'En exercice d\'alternance, l\'arrondi du curseur est réduit à 5 et le "step" du curseur (son et pause) est supprimé'
          ],
          fixed: [
            'Le surlignage de séance différée ne s’efface plus si le particulier a commencé le dernier exercice',
            'Correction d’un bug qui faisait apparaître les paramètres double cartes son en modifiant la fenêtre des paramètres, alors qu’une seule carte son est active'
          ]
        }
      ]
    }
  },
  created() {
    this.getLocalStorageSave()
  },
  updated() {
    this.SaveToLocalStorage()
    this.isChangelogEmpty()
  },
  methods: {
    resetSave () {
      localStorage.setItem('changelog', JSON.stringify(this.unchangedChangelogs))
      this.changelogActive = true
    },
    SaveToLocalStorage () {
      localStorage.setItem('changelog', JSON.stringify(this.changelogs))
    },
    getLocalStorageSave () {
      if (localStorage.getItem('changelog') !== 'null') {
        this.changelogs = JSON.parse(localStorage.getItem('changelog'))
        this.changelogActive = true
      } else {
        this.changelogActive = false
        console.log('no changelog saved')
      }
    },
    deleteChangelog () {
      localStorage.removeItem('changelog')
      this.changelogs = []
      this.changelogActive = false
    },
    deletelogCurrentChangelog (changelogId) {
      this.changelogs.splice(changelogId, 1)
    },
    deleteSection (sectionType, changelogIndex) {
      delete this.changelogs[changelogIndex][`${sectionType}`]
    },
    deleteLine (changelogIndex, type, index) {
      if (this.changelogs[changelogIndex][`${type}`].length <= 1) {
        this.changelogs[changelogIndex][`${type}`][index] = null
        } else {
        this.changelogs[changelogIndex][`${type}`].splice(index, 1)
      }
    },
    newLine (changelogIndex, type, index) {
      this.changelogs[changelogIndex][`${type}`].splice(index, 0, '')
    },
    isChangelogEmpty () {
      if (this.changelogs.length === 0) {
        this.changelogActive = false
      }
    },
    createNewChangelog (changelogIndex) {
      this.changelogActive = true
      this.changelogs.splice(changelogIndex, 0, {
        ver: 'v0.0.0',
        date: this.getCurrentDate()
      })
    },
    createNewSection (changelogId, type) {
      this.changelogs[changelogId][`${type}`] = ['']
    },
    getCurrentDate () {
      let today = new Date()
      let dd = String(today.getDate()).padStart(2, '0')
      const months = [
        "Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"
      ]
      let mm = months[today.getMonth()]
      let yyyy = today.getFullYear()
      return `${dd} ${mm} ${yyyy}`
    }
  }
}
</script>

<style scoped>
.container {
  margin: 0 250px;
}

.topBtn {
  margin-bottom: 1rem;
  padding: 0.5rem;
  border-radius: 10px;
  border: none;
  background: #5968E5;
  color: white;
  font-weight: bold;
  cursor: pointer;
}

.changelog__section-body {
  display: flex;
  flex-direction: column;
  justify-content: left;
  text-align: left;
  flex: 1;
  padding: 0 1rem;
  background: #F5F5F5;
  border-radius: 10px;
}

.version {
  font-size: 20px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-weight: 700;
  background: #F5f5f5;
  border: none;
  margin: 1rem 0;
  width: 70px;
}

.date {
  font-size: 16px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-weight: 700;
  background: #F5f5f5;
  border: none;
  width: 90%;
}

.separator {
  width: 1.5px;
  height: 20px;
  background: black;
  margin: 0 1rem;
}

.changelog__section-container {
  margin-bottom: 0.5rem;
}

.changelog__section-container-btn {
  border: none;
  padding: 1rem;
  border-radius: 8px;
  color: white;
  font-weight: 1000;
  line-height: 10px;
  letter-spacing: 2px;
}

li {
  margin-bottom: 8px;
}

.text {
  border: none;
  background: #F5F5F5;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 15px;
  width: 95%;
}

.text:hover + .delete {
  display: block;
}

.add {
  background: #4CAF50;
  width: 105px;
}

.changed {
  background: #FF9800;
  width: 110px;
}

.fixed {
  background: #2196F3;
  width: 115px;
}

.removed {
  background: #f44336;
  width: 130px;
}

/* edit mode */

.delete {
  cursor: pointer;
  display: none;
}

.delete:hover {
  display: block;
}

.changelog__section-container-btn:hover + .delete {
  display: block;
}

.editable:focus {
  outline: none;
}

.hide {
  display: none;
  position: relative;
  bottom: 30px;
  margin-top: -15px;
  margin-bottom: -17px;
  text-align: center;
  /* background: red; */
}

.hide:hover {
  display: block;
}

.addNewSection {
  width: 100%;
  height: 40px;
  margin: 0.5rem 0;
  /* background: green; */
}

.addNewSection:hover + .hide {
  display: block;
}

.hideDeleteChangelog {
  display: none;
  cursor: pointer;
  position: absolute;
}

.hideDeleteChangelog:hover {
  display: block;
}

.deleteCurrentChangelog {
  width: 30px;
  height: 100%;
}

.deleteCurrentChangelog:hover + .hideDeleteChangelog {
  display: block;
}

textarea {
  resize: none;
  border: none;
  background: #F5F5F5;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 15px;
  margin-bottom: -8px;
  border: 2px solid #F5F5F5;
}

textarea:focus {
  outline: none;
  border: 2px solid #5968E5;
}

@media only screen and (max-width: 600px) {
  .container {
    margin: 0;
  }

  .circle, .circleHide {
    left: 74px;
  }

  .changelog__section-left {
    display: none;
  }

  .changelog__section-body {
    margin: 0 1rem;
  }
}

@media only screen and (max-width: 1000px) {
  .container {
    margin: 0;
  }

  .circle, .circleHide {
    left: 142px;
  }

  .changelog__section-left {
    width: 120px;
    margin-right: 2rem;
  }
}

/* Animations & Transitions */
/* 1. declare transition */
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. declare enter from and leave to state */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translateY(30px, 0);
}

/* 3. ensure leaving items are taken out of layout flow so that moving
      animations can be calculated correctly. */
.fade-leave-active {
  position: absolute;
}
</style>