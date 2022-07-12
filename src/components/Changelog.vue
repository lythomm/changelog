<template>
  <div class="container">

    <h1>Changelog v0.1.0</h1>

    <div class="changelog__section" v-for="changelog in changelogs" :key="changelog.id">
      <div class="changelog__section-left">
        <div class="changelog__section-date">{{changelog.date}}</div>
        <div class="circle" />
        <div class="circleHide" />
      </div>
      <div class="changelog__section-body">
        <div style="display: flex; align-items: center">
          <h3 style="color: #5968E5; margin-right: 0.5rem">v{{changelog.ver}}</h3>
          <p class="mobile" style="font-weight: bold">{{changelog.date}}</p>
        </div>
        <div class="changelog__section-container">
          <div v-if="changelog.added">
            <button class="changelog__section-container-btn add">AJOUTÉ</button>
            <ul v-for="add in changelog.added" :key="add">
              <li>
                {{add}}
              </li>
            </ul>
          </div>
        </div>
        <div class="changelog__section-container">
          <div v-if="changelog.changed">
            <button class="changelog__section-container-btn changed">CHANGÉ</button>
            <ul v-for="change in changelog.changed" :key="change">
              <li>
                {{change}}          
              </li>
            </ul>
          </div>
        </div>
        <div class="changelog__section-container">
          <div v-if="changelog.fixed">
            <button class="changelog__section-container-btn fixed">CORRIGÉ</button>
            <ul v-for="fix in changelog.fixed" :key="fix">
              <li>
                {{fix}}
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Change-log',
  data () {
    return {
      changelogs: [
        {
          id: 1,
          ver: '2.46.3',
          date: '5 Juillet 2022',
          added: [
            'Le champ “email” est désomais obligatoire lors de la création d’un patient',
            'En version dégradé, ajout d’un bouton permettant de revenir en mode Normale'
          ],
          changed: [
            'La popup de choix de contenu met désormais un temps raisonnable à disparaître'
          ],
          fixed: [
            'La popup de choix de contenu met désormais un temps raisonnable à disparaître',
            'Correction d’un bug qui empêchait de lancer un exercice en mode hors-dossier',
            'Correction d’un bug où l’exportation des textes de contenus ne fonctionnait pas'
          ]
        },
        {
          id: 2,
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
      ]
    }
  }
}
</script>

<style scoped>
.container {
  margin: 0 120px;
}

.changelog__section {
  display: flex;
  margin-bottom: 1rem;
}

.changelog__section-left {
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
}

.circleHide {
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
}

.changelog__section-body {
  display: flex;
  flex-direction: column;
  justify-content: left;
  text-align: left;
  flex: 1;
  padding: 0 1rem;
  margin-right: 2rem;
  background: #F5F5F5;
  border-radius: 10px;
}

.changelog__section-container {
  margin-bottom: 0.5rem;
}

.mobile {
  display: none;
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

.add {
  background: #4CAF50;
  width: 100px;
}

.changed {
  background: #FF9800;
  width: 110px;
}

.fixed {
  background: #2196F3;
  width: 110px;
}

@media only screen and (max-width: 600px) {
  .container {
    margin: 0;
  }

  .mobile {
    display: unset;
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