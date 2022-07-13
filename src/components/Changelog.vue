<template>
  <div class="container" v-on:keyup.enter="SaveToLocalStorage">

    <h1>Changelog v0.1.0</h1>
  
    <div>
      <button class="topBtn" @click="SaveToLocalStorage" style="margin-right: 1rem">Sauvegarder</button>
      <button class="topBtn" v-if="!editMode && changelogSaved" @click="editMode = true">Éditer</button>
      <button class="topBtn" v-if="editMode && changelogSaved" @click="editMode = false">Stop Éditer</button>
      <button class="topBtn" v-if="changelogSaved" @click="deleteChangelog" style="margin-left: 1rem">Supprimer le changelog</button>
    </div>

    <div v-if="changelogSaved">
      <div class="changelog__section" v-for="(changelog, changelogIndex) in changelogs" :key="changelog.id">
        <!-- <div class="changelog__section-left">
          <div class="changelog__section-date">{{changelog.date}}</div>
          <div class="circle" />
          <div class="circleHide" />
        </div> -->
        <div class="changelog__section-body">
          <div style="display: flex; align-items: center">
            <h3 style="color: #5968E5">v{{changelog.ver}}</h3>
            <div class="separator" />
            <p style="font-weight: bold">{{changelog.date}}</p>
          </div>
          <div class="changelog__section-container">
            <div v-if="changelog.added">
              <div style="display: flex; align-items: center">
                <CloseCircle @click="deleteSection('added', changelogIndex)" v-if="editMode" :size="30" fillColor="red" class="delete" />
                <button class="changelog__section-container-btn add">AJOUTÉ</button>
              </div>
              <ul v-for="(add, index) in changelog.added" :key="index">
                <li>
                  <div style="width: 700px" ref="container">
                    <textarea rows="1" @focus="resizeTextarea" @keyup="resizeTextarea" v-model="changelog.added[index]" />
                  </div>
                </li>
              </ul>
            </div>
          </div>
          <div class="changelog__section-container">
            <div v-if="changelog.changed">
              <div style="display: flex; align-items: center">
                <CloseCircle @click="deleteSection('changed', changelogIndex)" v-if="editMode" :size="30" fillColor="red" class="delete" />
                <button class="changelog__section-container-btn changed">CHANGÉ</button>
              </div>
              <ul v-for="(change) in changelog.changed" :key="change">
                <li class="editable" contenteditable="true" @blur="SaveToLocalStorage">
                  {{change}}
                </li>
              </ul>
            </div>
          </div>
          <div class="changelog__section-container">
            <div v-if="changelog.fixed">
              <div style="display: flex; align-items: center">
                <CloseCircle @click="deleteSection('fixed', changelogIndex)" v-if="editMode" :size="30" fillColor="red" class="delete" />
                <button class="changelog__section-container-btn fixed">CORRIGÉ</button>
              </div>
              <ul v-for="fix in changelog.fixed" :key="fix">
                <li>
                  {{fix}}
                </li>
              </ul>
            </div>
          </div>
          <div class="changelog__section-container">
            <div v-if="changelog.removed">
              <div style="display: flex; align-items: center">
                <CloseCircle @click="deleteSection('removed', changelogIndex)" v-if="editMode" :size="30" fillColor="red" class="delete" />
                <button class="changelog__section-container-btn removed">SUPPRIMÉ</button>
              </div>
              <ul v-for="remove in changelog.removed" :key="remove">
                <li>
                  {{remove}}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-else>
      Commencer a écrire mon changelog
    </div>

  </div>
</template>

<script>
import CloseCircle from 'vue-material-design-icons/CloseCircle.vue'

export default {
  name: 'Change-log',
  components: {
    CloseCircle
  },
  data () {
    return {
      changelogSaved: false,
      editMode: false,
      changelogs: [
        {
          id: 0,
          ver: '2.46.3',
          date: '5 Juillet 2022',
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
          id: 1,
          ver: '2.46.0',
          date: '4 Juillet 2022',
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
      ],
      newChangelogs: []
    }
  },
  created() {
    this.getLocalStorageSave()
  },
  mounted() {
    this.$nextTick(()=>{
    this.$refs.container.forEach( ta => {
        ta.firstChild.dispatchEvent(new Event("keyup"));
      });
    });
  },
  updated() {
    this.SaveToLocalStorage()
  },
  methods: {
    resizeTextarea(e) {
      let area = e.target;
      area.style.overflow = 'hidden';
      area.style.height = area.scrollHeight + 'px';
    },
    SaveToLocalStorage() {
      console.log(this.changelogs)
      localStorage.setItem('changelog', JSON.stringify(this.changelogs))
    },
    getLocalStorageSave() {
      if (localStorage.getItem('changelog') !== null) {
        this.changelogs = JSON.parse(localStorage.getItem('changelog'))
        this.changelogSaved = true
        // console.log(changelog)
      } else {
        this.changelogSaved = false
        console.log('no changelog saved')
      }
    },
    deleteChangelog() {
      localStorage.removeItem('changelog')
      this.changelogs = null
      this.changelogSaved = false
    },
    deleteSection(sectionType, changelogIndex) {
      this.changelogs[changelogIndex][`${sectionType}`] = null
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

.changelog__section {
  display: flex;
  margin-bottom: 1rem;
}

/* .changelog__section-left {
  width: 250px;
  padding: 1rem;
  border-right: 4px solid #5968E5;
  margin-right: 6rem;
  padding-right: -2rem;
  display: flex;
  justify-content: space-between;
}

.changelog__section-date {
  font-weight: 600;
  flex: 1;
} */

/* .circleHide {
  width: 40px;
  height: 40px;
  background: white;
  border-radius: 50%;
  position: absolute;
  left: 392px;
  margin-top: -18px;
  z-index: 0;
}

.circle {
  width: 40px;
  height: 40px;
  background: #5968E5;
  border-radius: 50%;
  position: absolute;
  left: 392px;
  margin-top: -10px;
  z-index: 1;
} */

.changelog__section-body {
  display: flex;
  flex-direction: column;
  justify-content: left;
  text-align: left;
  flex: 1;
  padding: 0 1rem;
  /* margin-right: 2rem; */
  background: #F5F5F5;
  border-radius: 10px;
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
  border: 2px solid #f5f5f5;
  cursor: pointer;
}

li {
  margin-bottom: 8px;
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
  margin-right: 10px;
}

.changelog__section-container-btn:hover {
  border: 2px solid #5968E5;
}

.editable:focus {
  outline: none;
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
</style>