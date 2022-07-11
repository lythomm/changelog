<template>
  <div>
    <h1>Changelog v0.0.1</h1>
    <div class="changelog__section">
      <div class="changelog__section-left">
        <div class="changelog__section-date">5 Juillet 2022</div>
        <div class="circle" />
        <div class="circleHide" />
      </div>
      <div class="changelog__section-body">
        <h3 style="color: #5968E5">v2.46.3</h3>
        <div class="changelog__section-container">
          <button class="changelog__section-container-btn add">AJOUTÉ</button>
          <ul>
            <li>
              Le champ “email” est désomais obligatoire lors de la création d’un patient
            </li>
            <li>
              En version dégradé, ajout d’un bouton permettant de revenir en mode Normale
            </li>
          </ul>
        </div>
        <div class="changelog__section-container">
          <button class="changelog__section-container-btn changed">CHANGÉ</button>
          <ul>
            <li>
              La popup de choix de contenu met désormais un temps raisonnable à disparaître            
            </li>
          </ul>
        </div>
        <div class="changelog__section-container">
          <button class="changelog__section-container-btn fixed">CORRIGÉ</button>
          <ul>
            <li>
              La popup de choix de contenu met désormais un temps raisonnable à disparaître
            </li>
            <li>
              Correction d’un bug qui empêchait de lancer un exercice en mode hors-dossier
            </li>
            <li>
              Correction d’un bug où l’exportation des textes de contenus ne fonctionnait pas
            </li>
          </ul>
        </div>
      </div>
    </div>
    <div class="changelog__section">
      <div class="changelog__section-left">
        <div class="changelog__section-date">4 Juillet 2022</div>
        <div class="circle" />
        <div class="circleHide" />
      </div>
      <div class="changelog__section-body">
        <h3 style="color: #5968E5">v2.46.0</h3>
        <div class="changelog__section-container">
          <button class="changelog__section-container-btn add">AJOUTÉ</button>
          <ul>
            <li>
              Message d’erreur si le contenu ne se télécharge pas côté particulier
            </li>
          </ul>
        </div>
        <div class="changelog__section-container">
          <button class="changelog__section-container-btn changed">CHANGÉ</button>
          <ul>
            <li>
              En exercice d'alternance, l'arrondi du curseur est réduit à 5 et le "step" du curseur (son et pause) est supprimé            
            </li>
          </ul>
        </div>
        <div class="changelog__section-container">
          <button class="changelog__section-container-btn fixed">CORRIGÉ</button>
          <ul>
            <li>
              Le surlignage de séance différée ne s’efface plus si le particulier a commencé le dernier exercice
            </li>
            <li>
              Correction d’un bug qui faisait apparaître les paramètres double cartes son en modifiant la fenêtre des paramètres, alors qu’une seule carte son est active 
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Change-log'
}
</script>

<style scoped>
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
  left: 272px;
  margin-top: -18px;
  z-index: 0;
}

.circle {
  width: 40px;
  height: 40px;
  background: #5968E5;
  border-radius: 50%;
  position: absolute;
  left: 272px;
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
</style>