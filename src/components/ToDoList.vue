<script setup>
import { reactive } from 'vue'
import Chose from "../Chose"
import ToDoListItem from "./ToDoListItem.vue";
import ToDoForm from "./ToDoForm.vue";

const url = "https://webmmi.iut-tlse3.fr/~pecatte/todos/public/1/todos";
const listeC = reactive([]); //reactive : données surveillée, réagit aux modifs, actualise,

function handlerFaire(idChose,fait,texteChose) {
    let myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    const fetchOptions = {
    method: "PUT",
    headers: myHeaders,
    body: JSON.stringify({id:idChose, libelle:texteChose, fait:!fait}),
    };

    fetch(url, fetchOptions)
        .then((dataJSON) => {
            console.log(dataJSON); //renvoie : {"status" : 0 ou 1}, savoir si ça a marché ou non
            getToDos() // récup des données de la BDD (dont la new "Chose" ajouté)
           })
        .catch((error) => console.log(error));
}

function handlerDelete(idChose) {
    const fetchOptions = {
    method: "DELETE", // je dis que je veux delete
    };
    fetch(url+"/"+idChose, fetchOptions)
        .then((dataJSON) => {
            console.log(dataJSON); //renvoie : {"status" : 0 ou 1}, savoir si ça a marché ou non
            getToDos() // récup des données de la BDD (dont la "Chose" suppr)
           })
        .catch((error) => console.log(error));
}

function handlerAdd(l) { // l ce qui est sortie du formulaire
    let myHeaders = new Headers();
    myHeaders.append("Content-Type", "application/json");
    const fetchOptions = {
    method: "POST", // je dis que je veux ajouter
    headers: myHeaders,
    body: JSON.stringify({ libelle : l }),
    };

    fetch(url, fetchOptions)
        .then((dataJSON) => {
            console.log(dataJSON); //renvoie : {"status" : 0 ou 1}, savoir si ça a marché ou non
            getToDos() // récup des données de la BDD (dont la new "Chose" ajouté)
           })
        .catch((error) => console.log(error));
}



function getToDos(){
    fetch(url)
    .then((dataJSON) => {
        console.log(dataJSON);
        let choses = dataJson //objet au format json
        listeC.splice(0,listeC.length) // vide la liste de 0 à la taille de ma liste avant actualisations sinon comme que des push il y a duplication
        for (let ch of choses){
            listeC.push(new Chose(ch.id,ch.libelle,ch.fait)) //sens orienté objet
        }
       })
    .catch((error) => console.log(error));
}

onMounted(()=>{ //execute une suele fois à la création du composants, notion de cycle de vie
    getToDos()
})
</script>


<template>
 <h3>Liste des choses à faire</h3>
 <br></br>
 <ToDoForm @addC="handlerAdd"></ToDoForm>
 <ul>
    <ToDoListItem
    v-for="(chose, index) in listeC"
    :key=[chose.id]
    :ch="chose"
    :handlerD="handlerDelete"
    :handlerF="handlerFaire"
    ></ToDoListItem>
 <li >

 </li>
 </ul>
</template>


<style scoped>
h3{
        margin:40px 0 0;
    }
</style>