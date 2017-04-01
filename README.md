
#Vue.js

## 1. Présentation

Vue.js est un système de data binding réactif avec une structure à base de composants imbriqués.
Pour le dire plus simplement, les changements dans les données du modèle se répercutent automatiquement dans la vue.
Chaque objet Vue est un composant, qui lui même contient d'autres composants.

> Chaque fin de chapitre propose des liens pertinents vers la [documentation officielle](https://vuejs.org/guide/)

---
 - https://vuejs.org/guide/overview.html
 - https://vuejs.org/api/
###Hello World
L'exemple le plus simple est un Hello World.
Dans le fichier HTML, Vue emploi la syntaxe en moustache pour lier les données à la vue.
> Les exemples sont éditables, il suffit de cliquer le bouton "Edit in JSFiddle"
<iframe width="100%" height="200" src="//jsfiddle.net/lionel_bijaoui/wha86s8g/embedded/html/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
Ensuite, dans le fichier JavaScript, il suffit de créer un composant Vue.
<iframe width="100%" height="220" src="//jsfiddle.net/lionel_bijaoui/wha86s8g/embedded/js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
et  voilà
<iframe width="100%" height="100" src="//jsfiddle.net/lionel_bijaoui/wha86s8g/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
Au minimum, un composant Vue standard nécessite un paramètre `data` et `el`.
Le premier est le modèle dans l'architecture MVC, et l'objet Vue lui-même est équivalent au contrôleur.
Le second permet à Vue de se lier à un élément du DOM pour interagir avec.
Il est possible de se passer de ces paramètres dans certain cas particulier. Par exemple, un composant utilisé comme un service n'a pas forcement besoin d'apparaitre dans la vue.

----------
 - [data](https://vuejs.org/api/#data)
 - [el](https://vuejs.org/api/#el)
 - https://vuejs.org/guide/instance.html
 - https://vuejs.org/guide/syntax.html#Interpolations

###Two-way binding
Jusque là, nous avons montré comment utiliser Vue pour modifier la vue à partir d'un modèle.
Mais Vue est bien sûr capable d'aller dans l'autre sens.

La syntaxe en moustache n'est pas la seul manière d’interagir avec la vue.
Vue utilise des directives sous la forme `v-*`.

Pour faire modifier les données du modèle à partir de la vue, nous allons utiliser la directive `v-model`.

> Utilisez les onglets pour passer du JavaScript à l'HTML et au résultat interactif

<iframe width="100%" height="210" src="//jsfiddle.net/lionel_bijaoui/jwcd5coh/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
La modification du texte dans le champ de texte se répercute dans le modèle qui à sont tour modifie la vue.

----------
 - [v-model](https://vuejs.org/api/#v-model)
 - https://vuejs.org/guide/syntax.html#Directives

###Listes
La syntaxe en moustache et `v-model` sont suffisant pour interagir avec une donnée simple, mais souvent, les données sont sous la forme de tableau ou collection.
C'est à ce moment que `v-for` entre en jeu. Cette directive permet de boucler sur un tableau (ou un objet).
<iframe width="100%" height="300" src="//jsfiddle.net/lionel_bijaoui/pbnde3h9/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

----------
 - [v-for](https://vuejs.org/api/#v-for)
###Interaction
Maintenant que nous savons comment lier les données à la vue dans les deux sens, il est temps de rendre la vue interactive avec `v-on`.
`v-on` va lier un évènement (`click` par exemple) à une fonction interne du composant Vue.
Les fonctions interne sont déclaré dans le paramètre `methods` et ont accès aux données du composant à travers `this`.
> Dans sa forme courte, `v-on:event` peut s'écrire `@event`

<iframe width="100%" height="320" src="//jsfiddle.net/lionel_bijaoui/fc05Lr1z/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

----------
 - [v-on](https://vuejs.org/api/#v-on)
 - https://vuejs.org/api/#methods
###Todo
Pour finir voilà un exemple de Todo qui mélange le tout. Il y a quelques observations intéressantes à faire :

 - `v-on:keyup.enter` : ici `v-on` utilise l'événement clavier `keyup` avec un modificateur `enter`. Les modificateur peuvent servir à limiter l'événement à une touche de clavier (comme dans ce cas) et/ou à empêcher la propagation de l'événement, entre autre.
 - `v-on:click="removeTodo($index)"` : la variable `$index` correspond à l'index de la boucle actuelle. Pour explicitement déclarer son nom, il est possible d'écrire `v-for="(index, item) in items"`.

<iframe width="100%" height="580" src="//jsfiddle.net/lionel_bijaoui/f8Ly4kpk/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

----------

 - https://vuejs.org/guide/list.html


----------


##2. Exercice

Nous allons créer un petit site en exploitant Vuejs. Le site affiche une liste d'utilisateur et de groupe, ainsi que quelques statistique simples. Toute ces données sont issu d'une API tournant avec CakePHP 3.

###Préparation
Ceci est plus une checklist qu'un vrai guide car je serais là pour porter assistance :

 1. installation de docker
 2. modification des fichiers HOST (ajout de `client.tuto_vue` et `api.tuto_vue` sur l'IP de docker)
 3. récupération du projet de base
 4. lancement du serveur avec `docker-compose up -d`

Une fois que le serveur de l'API tourne bien, nous pouvons commencer à créer notre application.
Le projet de base contient un fichier `index.html` dans `/volumes/www/client/app` et un fichier `script.js` dans le dossier `/js` adjacent.
L'index contient déjà les libs qui seront utile au projet :

 - Material Design
	 Pour la présentation et structure uniquement.
	 - [La police Roboto](https://www.google.com/fonts/specimen/Roboto)
	 - [Les icones Material](https://design.google.com/icons/) de Google
	 - [Material Design Lite](https://getmdl.io/)
 - [Keen-ui](http://josephuspaye.github.io/Keen-UI/)
	 Une collection de composant Vue, inspiré par le Material Design qui permettrons de gagner du temps pour l'exercice.
 - [Chart.js](http://www.chartjs.org/)
	Pour la visualisation des statistiques.
 - [Vue.js](https://vuejs.org/)
	En plus de Vue, nous allons utiliser 2 module officiels :
	 - [vue-router](https://github.com/vuejs/vue-router)
		Transforme Vue en vrai SPA, avec un routage complet.
	 - [vue-resource](https://github.com/vuejs/vue-resource)
		Simplifie la gestion des requêtes HTTP.
		
Il existe une [extension officielle pour Chrome](https://github.com/vuejs/vue-devtools), qui fourni une Vue Devtool très pratique pour visualiser son application. Pour l'activer, penser à ajouter `Vue.config.devtools = true;`.
![vue devtool screenshot](https://cdn.rawgit.com/vuejs/vue-devtools/master/media/screenshot.png)
###Composant racine et routage
Dans le fichier `script.js`, commençons par dire à Vue d'utiliser utilise vue-router et les composants de Keen UI.
```js
(function(window, undefined) {
	"use strict";
	//Active la Vue Devtools (optionnel)
	Vue.config.devtools = true;
	// Vue utilise désormais le module de routage
	Vue.use(VueRouter);
	// Vue utilise désormais les composants de Keen UI
	Vue.use(Keen);
})(window);
```
Le système de routage a besoin d'un composant principal pour fonctionner. `App` va jouer ce rôle.
```js
var App = Vue.extend();
```
`App` est un composant un peu particulier, car il est vide. Normalement `Vue.extend()` prend un objet en paramètre, mais nous verrons cela plus tard avec d'autres composants.

Créons le routeur et définissons les routes de l'application
```js
// Nous passons quelques options au routeur, mais n'y faites pas trop attention
var router = new VueRouter({
        linkActiveClass: "is-active",
        history: true
    });
// Le routage de l'application
// le plus important est de définir quel composant pour quelle route
router.map({
        '/users': {
            component: ListView, // L'url '/users' utilise le composant Listview.
            name: "Utilisateurs", // Sera utilisé par le titre dynamique.
            type: "users" // Paramètre customisé accessible par '$route.type'.
        },
        '/groups': {
            component: ListView, // Même choses...
            name: "Groupes", // Entièrement optionnel mais j'en fais usage.
            type: "groups" // type aurait pu s'appeler "toto" ou autre...
        },
        '/stats': {
            component: StatsView, // '/stats' utilise le composant StatsView.
            name: "Statistiques", // Simplifie la prise en charge des traductions.
            type: "stats"
        }
    });
// redirige les URL inconnus vers /users
router.redirect({
    '*': '/users'
})
// maintenant l'application peut démarrer
router.start(App, '#app');
// ce code est très spécifique au module de routage
```

`router.start()`ancre `App` dans l’élément avec l'id "app". Il faut donc rajouter cet id dans `index.html`.
Tant qu'à éditer ce fichier autant ajouter une navigation. La directive `v-link` sert à la navigation.
Enfin, l'élément le plus important `<router-view></router-view>`, qui indique le point d’insertion des composants du routeur.
```html
<body id="app">
    <div class="mdl-tabs mdl-js-tabs mdl-js-ripple-effect">
        <div class="mdl-tabs__tab-bar">
	    <!--l'ancre n'a pas l'attribue "href" car v-link joue le même rôle-->
            <a class="mdl-tabs__tab" v-link="{ path: '/users'}">Users</a>
            <a class="mdl-tabs__tab" v-link="{ path: '/groups'}">Groups</a>
            <a class="mdl-tabs__tab" v-link="{ path: '/stats'}">Stats</a>
        </div>
    </div>
<!--c'est ici que les templates des composants s'insèrent-->
    <router-view></router-view>
```
`ListView` et `StatsView` n'existe pas et le routage ne marche pas encore. Nous allons les créer.

----------
 - [documentation de vue-router](http://router.vuejs.org/en/)
 - [v-link](http://router.vuejs.org/en/link.html)

----------
###Les composants `ListView` et `StatsView`
Pour l'instant, les composants utilisés par le routeur sont presque vide. Seul `template` défini l'HTML qui est utilisé.
```js
	var ListView = Vue.extend({
	    template: '<h1>{{$route.name}}</h1>' //Titre dynamique issue de la route
	});
	
	var StatsView = Vue.extend({
	    template: '<h1>{{$route.name}}</h1>'
	});
```
Désormais, l'application et le routage fonctionne. Rendez vous sur http://client.tuto_vue/.
Vous devriez voir ça :

![enter image description here](http://i.imgur.com/YACqPlJ.png)

Les onglets permettent de changer de page et chaque page affiche un titre.
Si tout marche, bravo ! Essayez de changer le contenu des templates des composants.


----------
 - [template](https://vuejs.org/api/#template)
 - [Vue.extend](https://vuejs.org/api/#Vue-extend)

----------
###Composant globaux
Jusque ici, nous avons créé des composants dont la porté est locale. Il faut en faire référence explicitement pour les utiliser.
Par exemple, je ne peux pas utiliser `ListView` dans `StatsView` sans définir `components`.
```js
// ce n'est qu'un exemple, inutile de copier ce code
var ListView = Vue.extend({ /* ... */ })

var StatsView= Vue.extend({
  template: '<list-view></list-view>...',
  components: {
    // <list-view> sera utilisable dans la template de StatsView
    'list-view ': ListView 
  }
})
```
Un des avantages des composants est la possibilité de les utiliser dans les templates et ainsi découper une application en une multitude de composant séparé et réutilisable.
Pour rendre un composant global, il faut le créer avec `Vue.component()`. Nous allons créer un premier composant, `list-item`, dont le rôle est d'afficher les informations de la liste.
```js
   Vue.component('list-item', {
       template: '<li class="mdl-list__item mdl-list__item--three-line">' +
       '<span class="mdl-list__item-primary-content">' +
       '<i class="material-icons mdl-list__item-avatar">account_circle</i>'+
       '</span>' +
       '</li>'
   });
```
Pour l'instant le composant n'est rien de plus qu'une template fixe qui affiche un icône. Insérons-le quand même dans la template de `ListView`.
```js
var ListView = Vue.extend({
    template: '<h1>{{$route.name}}</h1>'+ // Titre dynamique            
        // Contenu
        '<div class="mdl-grid">' +
            '<ul class="mdl-list" >' +
                // Notre custom componant "list-item"
                '<list-item></list-item>' +               
            '</ul>' +
        '</div>'
});
```
![enter image description here](http://i.imgur.com/x18SxkL.png)

Il est temps de faire appel à notre API pour obtenir les données.

----------
 - [components](https://vuejs.org/api/#components)
 - http://vuejs.org/guide/components.html#Using-Components

----------
###API et vue-resource
`ListView` va devoir stocker les données issus de la requête. C'est le rôle de `data` dans lequel se situe la variable `list` :
```js
var ListView = Vue.extend({
    template: '...',
    data: function() {
        return {
            list: []
        }
    }
});
```
Contrairement aux exemples simples, `data` est généré par une fonction qui lui renvoie un nouvel objet. Comme les composants peuvent être utilisé à de nombreux endroits, cela évite qu'il partage le même objet JavaScript.

Nous allons créer une fonction interne qui se chargera de récupérer les données. Les fonctions propres à un composant sont écrite dans `methods`. Créons la fonction `refreshList`.
```js
/* ListView */
template: {...},
data: function() {...},
methods: {
    refreshList: function() {
        // this.$set() permet de changer la valeur d'une variable interne
        // de manière explicite pour Vuejs.
        // Ici on la remet juste à zéro.
        this.$set('list', []);
        // vue-resource permet d'utiliser "$http" pour faire des requêtes      
        this.$http({
	        // l'url utilise '$route.type' défini dans la route.
	        // juste un exemple d'utilisation des paramètre de route customisé
	        url: 'http://api.tuto_vue/' + this.$route.type,
	        // requête de type GET
	        method: 'GET',
	        headers: { Accept: 'application/json' } 
	        // "$http" est une promise et profite de leur syntaxe
	    }).then(function(response) {
            // on sauvegarde la réponse du serveur dans "list"
            this.$set('list', response.data.data);
        }, function(response) {
            // en cas de pépin
            console.error(response);
        });
    }
}
```
Désormais, ListView a un moyen de récupérer et remplir `list`. Utilisons cette fontion quand le composant est chargé par la route. Un paramètre particulier à `vue-router` sert exactement à ça.
```js
/* ListView */
template: {...},
data: function() {...},
methods: {...},
// Spécifique au routeur "vue-router"
route: {
    data: function() {
	    // sera déclenché automatiquement par la route
        this.refreshList();
    },
}
```

> C'est surement un bon moment pour rappeler l'utilité de Vue Devtools sur Chrome qui permet de facilement  voir les valeurs internes des composants (entre autre)
![enter image description here](http://i.imgur.com/EJWHuYg.png)

Bien ! Maintenant que nous avons un tableau de valeur, nous allons l'utiliser pour boucler list-item avec `v-for` et transférer les valeurs au travers d'une "prop".
```js
/* ListView */
template: '<h1>{{$route.name}}</h1>' + // Titre dynamique            
    // Contenu
    '<div class="mdl-grid">' +
    '<ul class="mdl-list" >' +
    // list-item qui boucle et envoie la valeur dans la prop ":item"
    '<list-item v-for="item in list" :item="item"></list-item>' +
    '</ul>' +
    '</div>',
data: function() {...},
methods: {...},
route: {...}
```
A l'aide de cette "prop" nous pouvons envoyer la valeur au composant. Il faut bien sûr le configurer pour qu'il soit capable de l'utiliser. 
> Pour l'instant, le nombre d’icône devrais augmenter
![enter image description here](http://i.imgur.com/zEfPo7e.png)

```js
/* list-item */
// la template est mise à jour pour utiliser les données
// Vous êtes libre de faire autre chose
template: '<li class="mdl-list__item mdl-list__item--three-line">' +
    '<span class="mdl-list__item-primary-content">' +
    // v-if permet d'ajouter conditionnellement un élément
    // ici je l'utilise pour afficher le bon icône selon le type de donnée
    '<i class="material-icons mdl-list__item-avatar" v-if="item.first_name">account_circle</i>' +
    // v-else est le pendant de v-if et fonctionne comme vous imaginez
    '<i class="material-icons mdl-list__item-avatar" v-else>group_work</i>' +
    // v-text modifie le contenu de l'élément
    // similaire à l'interpolation {{ moustache }} 
    // mais offre de meilleures performances
    '<span v-if="item.first_name" v-text="item.first_name"></span>' +
    // les directives conditionnelle sont plus efficace quand placé en premier
    // cela évite à Vue de gérer des directives inutiles
    '<span v-else v-text="item.name"></span>' +
    '<span class="mdl-list__item-text-body" v-if="item.email" v-text="item.email"></span>' +
    '<span class="mdl-list__item-text-body" v-else v-text="item.created"></span>' +
    '</span>' +
    '</li>',
props: ['item']
```
Ici, je gère les conditions à partir des données. Une meilleure manière serait de vérifier le paramètre `type` de l'objet `$route`.
Essayez d'implementer une meilleure solution à l'aide de `v-if`, `v-else`, les `props` et `this.$route`.


----------


Nous allons continuer avec le composant `list-item` pour introduire une autre fonctionnalité de Vue : les valeurs calculées.
Je veux créer une variable "full_name" qui sera composé de "first_name" et "last_name". Grace à `computed` c'est plutôt facile.

```js
/* list-item */
template: 
...
// je peux maintenant faire ça
'<i class="material-icons mdl-list__item-avatar" v-if="full_name">account_circle</i>' +
...
// full_name est disponible comme n'importe qu'elle variable
'<span v-if="item.first_name" v-text="full_name"></span>' +
...,
props: ['item'],
computed: {
	// ceci est un exemple simple avec un get
	// il est possible de prendre en compte set/get
    full_name: function() {
        if (this.item.first_name || this.item.last_name) {
            return this.item.first_name + " " + this.item.last_name;
        } else {
            return undefined;
        }
    }
}
```
Nous en avons fini avec le composant `list-item`. De même, `ListView` fonctionne plutôt bien maintenant, en affichant une liste d’éléments récupéré de l'API.
Il est temps de s'occuper des statistiques.

----------
 - [methods](https://vuejs.org/api/#methods)
 - [$set](https://vuejs.org/api/#vm-set)
 - [$http](https://github.com/vuejs/vue-resource/blob/master/docs/http.md)
 - [route.data](http://router.vuejs.org/en/pipeline/data.html)
 - [props](https://vuejs.org/api/#props)
 - [v-if](https://vuejs.org/api/#v-if)
 - [computed](https://vuejs.org/api/#computed)

----------
###Doughnut et bar
De la même manière que `list-item`, nous allons créer 2 nouveaux composants globaux `doughnut-chart` et `bar-chart`. Leur données internes sont initialisées dans `data` et dépendent du type de valeur utilisé. `doughnut-chart` expose une `props` "element" qui sera utile plus tard.
```js
Vue.component('doughnut-chart', {
    template: '<canvas width="100%" height="100%"></canvas>',
    data: function() {
        return {
            labels: [],
            data: []
        }
    },
    props: ["element"]
 });
Vue.component('bar-chart', {
    template: '<canvas width="100%" height="100%"></canvas>',
    data: function() {
        return {
            users: [],
            groups: [],
            os: []
        }
    }
});
```
Mettons à jour la template de `StatView`. Les éléments important sont `<doughnut-chart element="group">`, `<bar-chart>` et `<doughnut-chart element="os">`.
Le reste peut être changé selon votre goût. J'ai choisi de les placer dans des "cards" et dans une grille responsive.
```js
var StatsView = Vue.extend({
    template: '<h1>{{$route.name}}</h1>' +
        '<div class="mdl-grid">' +
        '<div class="mdl-cell mdl-cell--6-col-phone mdl-cell--4-col mdl-card mdl-shadow--2dp"><div class="mdl-card__title"><doughnut-chart element="group"></doughnut-chart></div></div>' +
        '<div class="mdl-cell mdl-cell--6-col-phone mdl-cell--4-col mdl-card mdl-shadow--2dp"><div class="mdl-card__title"><bar-chart></bar-chart></div></div>' +
        '<div class="mdl-cell mdl-cell--6-col-phone mdl-cell--4-col mdl-card mdl-shadow--2dp"><div class="mdl-card__title"><doughnut-chart element="os"></doughnut-chart></div></div>' +
        '</div>'
});
```
![enter image description here](http://i.imgur.com/XaYoux2.png)

Ces deux composants feront appel à Chart.js qui a besoin d'un éléments du DOM pour fonctionner. Les composants Vue ont accès à leur DOM local avec `this.$el`.

Nous allons pour l'instant créer des graphiques avec de fausses données, juste pour vérifier que tout marche bien.
Le meilleur moment pour initialiser Chart.js est quand le DOM devient disponible pour le composant. Je ne vais pas rentrer en détail dans [le cycle de vie d'un composant Vue](https://vuejs.org/guide/instance.html#Lifecycle-Diagram) mais c'est une lecture recommandé pour bien comprendre Vue.
`ready` correspond à cette étape, et c'est le meilleur moment pour interagir avec le DOM.
```js
Vue.component('doughnut-chart', {
    template: ...,
    data: function() {...},
    props: ...,
    ready: function() {
    //this.$el est disponible
        this.doughnutChart = new Chart(this.$el, {
            type: 'doughnut',
            data: {
                // fausse données pour l'instant
                labels: ["A", "B", "C"],
                datasets: [{
                // fausse données pour l'instant
                    data: [300, 50, 100],
                    backgroundColor: [
                        "rgb(63, 81, 181)",
                        "rgb(33, 150, 243)",
                        "rgb(0, 188, 212)"
                    ],
                    hoverBackgroundColor: [
                        "rgba(63, 81, 181, 0.7)",
                        "rgba(33, 150, 243, 0.7)",
                        "rgba(0, 188, 212, 0.7)"
                    ]
                }]
            },
            // des options de Chart.js
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                }
            }
        });
    }
});

Vue.component('bar-chart', {
    template: ...,
    data: function() {...},
    ready: function() {
        this.barChart = new Chart(this.$el, {
            type: 'bar',
            data: {
                // fausse données pour l'instant
                labels: ["A", "B", "C"],
                datasets: [{
                // fausse données pour l'instant
                    data: [65, 59, 80],
                    backgroundColor: "rgb(255, 152, 0)",
                    hoverBackgroundColor: "rgba(255, 152, 0, 0.7)"
                }]
            },
            // des options de Chart.js
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                },
                legend: {
                    display: false
                }
            }
        });
    }
});
```
![enter image description here](http://i.imgur.com/Ck9sA16.png)

Les graphiques devraient apparaitre. Il est temps de récupérer de vrai données.
Similaire à avant, j'utilise `$http`. Il est possible de faire une [factory ressource](https://github.com/vuejs/vue-resource/blob/master/docs/resource.md) réutilisable dans toute l'application mais j'ai préférer rester simple pour cet exercice.
```js
/* doughnut-chart */
ready: function() {
    this.doughnutChart = new Chart(this.$el, {...});
    this.$http({
	    url: 'http://api.tuto_vue/stats',
	    method: 'GET',
	    headers: { Accept: 'application/json' } 
    }).then(function(response) {
        // j'envoie la response à la fonction "parseStats()"        
        this.parseStats(response.data.entities, this.element);
    }, function(response) {});
},
methods: {
    // "parseStats()" se charge de traiter les données
    parseStats: function(stats, element) {
        var stats = stats;
        // remet les données à zéro
        this.data = new Array(3).fill(0);
        this.labels = [];

        for (var i = 0; i < stats.length; i++) {
            if (element == "group") {
                this.data[stats[i]["group_id"] - 1] += 1;
                this.labels[stats[i]["group_id"] - 1] = stats[i].group.name;
            } else if (element == "os") {
                this.data[stats[i].opsystems[0].id - 1] += 1;
                this.labels[stats[i].opsystems[0].id - 1] = stats[i].opsystems[0].name;
            }
        }
        this.doughnutChart.data.labels = this.labels;
        this.doughnutChart.data.datasets[0].data = this.data;
        this.doughnutChart.update();
    }
}
```
Les composants `doughnut-chart` chargent les données et se mettent à jour. Vous pouvez enlever les fausses données.

Pour `bar-chart`, nous faisons 3 requêtes simultanées. Une solution aurait pu être d'imbriquer les requêtes les unes dans les autres, mais nous allons faire autrement pour introduire une autre fonctionnalité de Vue : `watch`.
```js
/* bar-chart */
// chaque fois qu'un valeur listé dans "watch" change,
// la fonction associée se lance
watch: {
    // la fonction donne accès à la nouvelle et ancienne valeur
    'users': function(val, oldVal) {
        this.barChart.data.labels[0] = "Users";
        this.barChart.data.datasets[0].data[0] = val.length;
        this.barChart.update();
    },
    // Il est aussi possible d'observer une variable interne
    // avec this.$watch() pour plus de contrôle   
    'groups': function(val, oldVal) {
        this.barChart.data.labels[1] = "Groups";
        this.barChart.data.datasets[0].data[1] = val.length;
        this.barChart.update();
    },
    // Attention à ne pas créer de boucle infini
    'os': function(val, oldVal) {
        this.barChart.data.labels[2] = "Os";
        this.barChart.data.datasets[0].data[2] = val.length;
        this.barChart.update();
    },
},
ready: function() {
    this.barChart = new Chart(this.$el, {...});
    // ce code n'est pas très optimisé (DRY)
    // cherchez un moyen de mieux faire avec une fonction dans "methods"
    // ou une factory ressource
    this.$http({
	    url: 'http://api.tuto_vue/users',
	    method: 'GET', 
	    headers: { Accept: 'application/json' } 
    }).then(function(response) {
        this.$set('users', response.data.data);
        console.log('users', response.data.data);
    }, function(response) {});

    this.$http({
	    url: 'http://api.tuto_vue/groups',
	    method: 'GET',
	    headers: { Accept: 'application/json' }
    }).then(function(response) {
        this.$set('groups', response.data.data);
        console.log('groups', response.data.data);
    }, function(response) {});

    this.$http({
	    url: 'http://api.tuto_vue/opsystems',
	    method: 'GET',
	    headers: { Accept: 'application/json' }
    }).then(function(response) {
        this.$set('os', response.data.data);
        console.log('os', response.data.data);
    }, function(response) {});
}
```
L'application répond maintenant aux besoins basiques de l'exercice.
Vous avez du remarquer que les composants `doughnut-chart` font une requête à la même ressource, se qui représente un gaspillage de bande passante.
Plus il y a de graphique sur la page, plus il y aura de requête. De même, les composants sont très spécifique à `StatsView`, et ne sont pas très réutilisable dans un autre contexte.

Essayer d'utiliser `StatsView`, `watch` et les `props` pour créer un meilleur système.

----------
 - [ready](https://vuejs.org/api/#ready)
 - [watch](https://vuejs.org/api/#watch)
 - [$watch](https://vuejs.org/api/#vm-watch)

----------
### Touches finale
Maintenant que l'application est bien en place, il est temps de faire un petit travail de présentation. Tout ce qui suit n'est là que pour donner un exemple et présenter d'autres éléments de l'[API de Vue](https://vuejs.org/api/).
####Cacher la précompilation
Vous avez du remarquer que lors du chargement initial, certain éléments comme `{{$route.name}}` apparaissent subrepticement. Cela vient du fait que pendant le chargement de Vue et du routeur, la page n'est pas encore compilé.
Heureusement, la directive `v-cloak` permet de marquer les composants Vue qui n'ont pas fini leur interpolations; ici, le composant principal.
Avec un peu de CSS, on peut cibler cet élément pour le cacher, et appliquer une simple transition sur le body.

> Attention à ne pas confondre `v-cloak` avec `v-if` ou `v-show`

```html
    <style type="text/css">
    body{
        opacity: 1;
        transition: all .3s ease-out;
    }
    [v-cloak] {
        opacity: 0;
    }
    </style>
```
----------
 - [v-cloak](https://vuejs.org/api/#v-cloak)

----------
####Barre de progression
Durant le chargement des données du composant `ListView`, l'utilisateur accueilli par une liste vide. Pour faire communiquer à l'utilisateur que l'application attends des données, on peut faire apparaitre une barre de progression.
Le composant `<ui-progress-linear>` de Keen UI est une éventuelle solution. Sa prop `show` permet d'afficher ou de cacher l'élément avec une transition. `v-show` aurait aussi pu être employé si cette fonctionnalité n'était pas déjà présente dans ce composant.
`show` vérifie la condition `"list.length<=0"`, qui permet simplement de vérifier si la liste est vide. On peut imaginer un meilleur système qui prend en compte la réussite de la requête au serveur pour la masquer en cas d’échec.
```js
var ListView = Vue.extend({
	template:
	 // Barre d'avancement
	'<ui-progress-linear :show="list.length<=0" color="primary"></ui-progress-linear>' +
```
----------
 - [v-show](https://vuejs.org/api/#v-show)

----------
####Création d'utilisateurs/groupes (modale et formulaire)
J'ai ajouter un bouton qui déclenche une modale qui contient un formulaire qui pourrais servir à créer un nouveau groupe. C'est un simple exemple et vous êtes libre de faire différemment.
```js
/* ListView */
template: 
// Titre dynamique
'<h1>{{$route.name}}</h1>' +
// Contenu (amélioré avec une grille responsive)
'<div class="mdl-grid"><div class="mdl-cell mdl-cell--10-col-tablet mdl-cell--2-offset-tablet mdl-cell--6-col-desktop mdl-cell--4-offset-desktop ">' +
    '<ul class="mdl-list" >' +
        // Custom componant list-item qui boucle
        '<list-item v-for="item in list" :item="item"></list-item>' +
        // Bouton déclencheur de la modale
        // v-show cache l’élément
        '<li v-show="list.length > 0"><ui-fab @click="showmodal = true" color="default" icon="playlist_add" tooltip="Ajouter un element" tooltip-position="right middle"></ui-fab></li>' +
    '</ul>' +
'</div></div>' +
// Modale
'<ui-modal header="Ajouter un element" :show.sync="showmodal">' +
    // Contenu de la modale
    // Exemple de formulaire avec validation au blur
    '<ui-textbox v-if="$route.type == \'groups\'" label="Nom du groupe" name="name" type="text" validation-rules="required" :value.sync="info.name"></ui-textbox>' +
    '<ui-textbox v-if="$route.type == \'users\'" label="Username" name="username" type="text" validation-rules="required" validate-on-blur  :value.sync="info.username"></ui-textbox>' +
    '<ui-textbox v-if="$route.type == \'users\'" label="Email" name="email" type="email" validation-rules="required|email" validate-on-blur :value.sync="info.email"></ui-textbox>' +
    '<ui-select v-if="$route.type == \'users\'" name="group" label="Groupe" validation-rules="required" validate-on-blur :options="[{text:\'umami\'},{text:\'satt\'},{text:\'chu\'}]" placeholder="Choisir le groupe dans la liste" ></ui-select>' +
    '<ui-autocomplete v-if="$route.type == \'users\'" label="OS"  validation-rules="required" validate-on-blur :suggestions.once="[\'windows\',\'macosx\',\'gnu/linux\']" :min-chars.once="0" :value.sync="info.os" name="os" placeholder="Choisir un OS" help-text="Commencer à taper pour voir les suggestion"></ui-autocomplete>' +
    '<ui-textbox v-if="$route.type == \'users\'" label="Nom" name="name" type="text" :value.sync="info.first_name"></ui-textbox>' +
    '<ui-textbox v-if="$route.type == \'users\'" label="Prénom" name="name" type="text" :value.sync="info.last_name"></ui-textbox>' +
    // Modal Footer
    '<div slot="footer">' +
	    // Valider ne fait rien, mais un "@click" pourrais déclencher la fonction
	    // qui envoie la bonne requête PUT
        '<ui-button color="primary">Valider</ui-button>' +
        '<ui-button @click="showmodal = false">Fermer</ui-button>'+
    '</div>'+
'</ui-modal>',
data: function() {
    return {
        list: [],
        info: {
            // Stocke les informations du formulaire
            'os': "",
            'name': "",
            'username': "",
            'email': "",
            'first_name': "",
            'last_name': "",
            'group': ""
        },
        // Visibilité de la modale
        showmodal: false
    }
},
```



----------

##3. L'étape suivante
###Webpack & Vuejs
Webpack est un module bundler, c'est à dire une système qui prend de nombreux fichiers et les compactes intelligemment.
Contrairement à Grunt, Webpack fonctionne à partir d'un point d'entré puis va suivre la structure des import/require pour continuer son cheminement. Selon le type de fichier, les paramètres et la configuration de Webpack, les fichiers vont subir un traitement quand ils sont appelé.
Le paramétrage de Webpack est complexe et mérite un exercice de lui même.

Heureusement, grâce à l'aide de l'outil [vue-cli](https://github.com/vuejs/vue-cli), il est très simple de créer un projet Webpack préparé pour Vue.
L'avantage principal de cette méthode, dans le contexte de Vue, est la possibilité d'écrire des fichiers `.vue`.
Leur structure permet d'écrire des composant Vue avec cette syntaxe.
```
<!-- Hello.vue (basé sur le Hello World) -->
<template>
	<div>{{ message }}</div>
</template>
<script>	   
	export default {
		data: {
		     message: 'Hello Vue.js!'
		}
	}
</script>
```


```
<!-- ListView.vue (basé sur le composant créé dans l'exercice) -->
<!-- tout ce qui se situe dans <template> s'écrit comme de l'HTML. Plus besoin de concaténation et d’échapper certain caractères-->
<template>
    <ui-progress-linear :show="list.length<=0" color="primary"></ui-progress-linear>
    <h1>{{$route.name}}</h1>
    <div class="mdl-grid">
        <div class="mdl-cell mdl-cell--10-col-tablet mdl-cell--2-offset-tablet mdl-cell--6-col-desktop mdl-cell--4-offset-desktop ">
            <ul class="mdl-list">
                <list-item v-for="item in list" :item="item"></list-item>
                <li v-show="list.length > 0">
                    <ui-fab @click="showmodal = true" color="default" icon="playlist_add" tooltip="Ajouter un element" tooltip-position="right middle"></ui-fab>
                </li>
            </ul>
        </div>
    </div>
    <ui-modal header="Ajouter un element" :show="showmodal">
        <ui-textbox v-if="$route.type == 'groups'" label="Nom du groupe" name="name" type="text" validation-rules="required" :value="info.name"></ui-textbox>
        <ui-textbox v-if="$route.type == 'users'" label="Username" name="username" type="text" validation-rules="required" validate-on-blur :value="info.username"></ui-textbox>
        <ui-textbox v-if="$route.type == 'users'" label="Email" name="email" type="email" validation-rules="required|email" validate-on-blur :value="info.email"></ui-textbox>
        <ui-select v-if="$route.type == 'users'" name="group" label="Groupe" validation-rules="required" validate-on-blur :options="[{text:'umami'},{text:'satt'},{text:'chu'}]" placeholder="Choisir le groupe dans la liste"></ui-select>
        <ui-autocomplete v-if="$route.type == 'users'" label="OS" validation-rules="required" validate-on-blur :suggestions="['windows','macosx','gnu/linux']" :min-chars="0" :value="info.os" name="os" placeholder="Choisir un OS" help-text="Commencer à taper pour voir les suggestion"></ui-autocomplete>
        <ui-textbox v-if="$route.type == 'users'" label="Nom" name="name" type="text" :value="info.first_name"></ui-textbox>
        <ui-textbox v-if="$route.type == 'users'" label="Prénom" name="name" type="text" :value="info.last_name"></ui-textbox>
        <div slot="footer">
            <ui-button color="primary">Valider</ui-button>
            <ui-button @click="showmodal = false">Fermer</ui-button>
        </div>
    </ui-modal>
</template>

<!-- tout ce qui se situe dans <script> s'écrit comme du JavaScript
// La grosse différence est qu'il faut suivre la convention des modules
// avec "export default" et importer les composants utiles-->
<script>
// j'importe un composant utile
import listItem from './list-item.vue';

export default {
    data: function() {
        return {
            list: [],
            info: {
                'os': "",
                'name': "",
                'username': "",
                'email': "",
                'first_name': "",
                'last_name': "",
                'group': ""
            },
            showmodal: false
        }
    },
    route: {
        data: function() {
            this.refreshList();
        },
    },
    methods: {
        refreshList: function() {
            this.$set('list', []);
            // GET request
            this.$http({
                url: 'http://api.tuto_vue/' + this.$route.type,
                method: 'GET',
                headers: {
                    Accept: 'application/json'
                }
            }).then(function(response) {
                console.log('list', response.data.data);
                this.$set('list', response.data.data);
            }, function(response) {
                console.error(response);
            });
        }
    },    
    componants{
		listItem 
    }
}
</script>
```
Potentiellement, je peux écrire mon HTML en Jade ou Haml, le CSS en Sass ou Less, mon Javascript en TypeScript, Coffee Script ou en Ecmascript6. Je peux aussi éclater le composant en sous fichier au besoin.

Les possibilités sont grandes.
###Vers la V2
Pour cet exercice, j'ai pris soin d'utiliser le moins d'éléments de [l'API de Vue](https://github.com/vuejs/vue/wiki/1.0.0-binding-syntax-cheatsheet) qui ne se retrouveront pas dans [la future V2](https://github.com/vuejs/vue/issues/2873).
Cette version 2 n'introduit pas de grand bouleversement dans l'API mais ouvre de nouvelles opportunité intéressantes (pré-rendu coté serveur, virtual DOM) et une meilleur performance.
Le plus gros changement tourne autour des events. Il ne sera plus possible de diffuser un évènement à travers un arbre de composant (\$dispatch et \$broadcast).
Il est plutôt recommander de créer un composant 'bus' qui se charge de recevoir les events. Pour les events plus globaux, il est conseillé d'utiliser l'architecture Flux.
###Architecture Flux
L'architecture Flux cherche à simplifier la gestion des états (states) d'un application.
L'idée globale est qu'un state ne peut pas directement être modifié et que les modifications vont dans un seul sens (d'où le flux). Cela facilite le débugage et simplifie le raisonnement au sujet des évènements.

![enter image description here](https://cdn.rawgit.com/vuejs/vuex/master/docs/en/vuex.png)

Cette architecture marche très bien avec Vue et à été implémenter avec [vuex](https://github.com/vuejs/vuex).
Vuex couplé avec la devtool officielle permet même de "remonter dans le temps".

![vuex time travel](https://cdn.rawgit.com/vuejs/vue-devtools/master/media/demo.gif)
<iframe width="560" height="315" src="https://www.youtube.com/embed/l1KHL-TX3qs" frameborder="0" allowfullscreen></iframe>
