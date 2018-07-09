@title[Introduction]
![logo](assets/images/rx_logo.png) 

## @color[#cc6699](RxJava 2.0)
Reactive Extensions for the JVM
---

RxJava offre une manière élégante pour implémenter du code asynchrone.
* Callback : Complexité dans l'enchainement de callBack (Callback Hell)

```java
service.getData(dataA -> {
    service.getData(dataB -> {
        service.getData(dataC -> {
            // ...
        });
    });        
});
```
 * Future : Peuvent bloquer le code trop tôt ( appel à la méthode get) 
---

RxJava est basé sur le patron de conception Observateur :
L'objet observé envoie un signal à des composants qui jouent le rôle d'observateurs.  
 En cas de notification, les observateurs effectuent alors l'action adéquate en fonction des informations qui parviennent depuis les modules qu'ils observent.  
 
![observer_pattern](assets/images/observer.png)   

---
Un Observable peux être compris comme un Runnable, il va contenir une méthode qui va être executé dans un Thread différent.

```java
Observable<User> userObservable = Observable.create(new Observable.OnSubscribe<User>() {
            @Override
            public void call(Subscriber<? super Object> subscriber) {
                 
                //je peux par exemple faire un appel reseau bloquant
                User user = webservice.getUser("florent37");
 
                //une fois l'objet récupéré, il faut l'envoyer au notifieur,
                subscriber.onNext(user);
 
                //puis dire au notifieur que nous avons finit
                subscriber.onCompleted();
            }
        });
```
---
@div[left-100]
Une fois nos opérations longues terminées, nous souhaitons récupérer le résultat, pour cela nous utiliserons des Subscribe  
@divend
```java
userObservable.subscribe(new Subscriber<User>(){
     public void onNext(Object result) {
         //appelée à chaque fois que l'observable reçoit un objet User
     }
 
     public void onComplete() {
         //appelée lorsque l'observable a finit d'envoyer l'ensemble des objets User
     }
 
     public void onError(Throwable error) {
     }
});
```
---
## Le Manifeste Réactif  
https://www.reactivemanifesto.org/fr

- Disponible : Le système répond rapidement en toutes circonstances.
- Résilient : Le système reste disponible en cas d'erreur.
- Souple : Le système reste disponible indépendemment de la charge de travail
- Orientés messages (message-driven) : Le système utilise le passage de message asynchrones
entre ses composants afin de profiter de l'élasticité et  de la répartition des charges en appliquant la contre-pression ( back-pressure=)
---
## Reactive Streams
Une initiative qui essaye de normaliser le traitement asynchrone des flux avec une contre-pression non bloquante.  
RxJava2 implémente cette spécification.

![ractive](assets/images/reactive-streams-communication-flow.png)
---
## Contre Pression (Back-pressure)
![back-pressure](assets/images/backpressure.jpg)

---
## Types d'Observables

| Type         | Cas d'usage                                       |
|--------------|---------------------------------------------------|
| *Single*     | Opération asynchrone qui retourne 1 résultat      |
| *Maybe*      | Opération asynchrone qui retourne 0 ou 1 résultat |
| *Completable* | Opération asynchrone qui ne retourne aucun résultat. Indique la fin d'observation. | 
| *Observable* | Séquence de données sans contre-pression |
| *Flowable*| Séquence de données avec contre-pression |

---