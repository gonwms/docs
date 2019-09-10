# Firebase
## Instalación

1. crear projecto en https://console.firebase.google.com
1. **$ npm install firebase-tools -g** (solo la primera vez, ya lo tengo instalado creo)
1. **$ firebase init hosting**  - crea la estructura inicial de archivos
1. **$ firebase serve** - crea localhost en puerto 5000  - ctrl + c para terminar ejecución
1. **$ firebase deploy** - sube la app a internet 
1. Borrar todo lo de index.html menos los imports del Head
1. crear archivo app.js y agregarlo al final del index.html

## custom domain

1. Hacer verificación TXT y esperar.
2. Agregar DNS. **Siempre dejar los el Campo de nombre vacio.**. Si no anda probar con:  TTL 14400

## autenticación 

1. authentication: elegir métodos.



## links 
- [github.com/firebase](https://github.com/firebase)
- [chat hecho en firebase](https://github.com/othreecodes/friendlychat)




## firestore

### add
Agrega a un doc a la colección. Encambio. set() y update() modifician el contenido del doc.
Si la coleción no existe se crea sola.

```javascript
firebase.firestore().collection('lolo').add({
	first: "lolo",
	last: "lolololo",
	born: 1815
 })
 .then(function(docRef) {
	console.log("Document written with ID: ", docRef.id);
 })
 .catch(function(error) {
	console.error("Error adding document: ", error);
 });
```


### set & update

set() replacing all the child properties at the current location

update() can be use to selectively update only the referenced properties at the current location

```javascript
var doc = db.collection('dataModel').doc(clickedItemId);
doc.get()
	.then((object) => {
			doc.update({
					'priority':true
			})
	})
```

### get 

```javascript
 firebase.firestore().collection('dataModel').get()
 
 .then((querySnapshot) => {
	querySnapshot.forEach((doc) => {
	    console.log(`${doc.id} => ${doc.data()}`);
	})
 });
```

### delete

```javascript
doc.delete()
```

### onSnapshot

```javascript
db.collection('dataModel')

.where("soft_deleted", "==", false)
.where("account_ref_string", "==", "accounts/" + this.account_id)
.orderBy('date', "desc")

.onSnapshot((querySnapshot) => {
	querySnapshot.forEach((doc) => {
		console.log(doc);
		
		dataView.createAndInsertItem(doc)
	});
});

```
### docChanges

Tres estados posibles: **modified | added | removed**

```javascript
db.collection("cities").where("state", "==", "CA")
    .onSnapshot(function(snapshot) {
        snapshot.docChanges().forEach(function(change) {
            if (change.type === "added") {
                console.log("New city: ", change.doc.data());
            }
            if (change.type === "modified") {
                console.log("Modified city: ", change.doc.data());
            }
            if (change.type === "removed") {
                console.log("Removed city: ", change.doc.data());
            }
        });
    });
```
### batch
Ejemplo que me pasó Fer
```javascript
updateAllCampaignsDate: function(newValue, field, campaigns){
          var batch = db.batch();
          campaigns.forEach((camp) => {
            var temp = db.collection("campaigns").doc(camp.id);
            batch.update(temp, {[field]: newValue});
          });
          batch.commit();
        },
```