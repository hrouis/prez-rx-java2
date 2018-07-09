@title[Introduction]
![logo](assets/logo.png) 

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

RxJava est basé sur le patron de conception Observable :   

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