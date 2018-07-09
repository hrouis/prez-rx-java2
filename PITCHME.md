@title[Introduction]
![logo](assets/logo.png) 

## @color[#cc6699](RxJava 2.0)
Reactive Extensions for the JVM
---

RxJava offre une manière élégante pour implémenter du code asynchrone.
Callback : Complxité dans l'enchainement de callBack 

```java
service.getData(dataA -> {
    service.getData(dataB -> {
        service.getData(dataC -> {
            // ...
        });
    });        
});
```

---