# InteractJS

**InteractJS** es simplemente una manera de crear tus proyectos mas facilmente en los que necesitas tener como base una "base de datos" local json sin necesidad de estar pensando en crear una y otra vez el tipico **crud**

Para utilizar **InteractJS** hay que instancear la clase **Interact** y definir con **dbPath** la ruta de tu **db.json**

Para cargar archivos a su proyecto es necesario definir una propiedad llamada **destFiles** especificando la ruta destino

Como definir en numero de resultados que se recibiran por pagina con **totalItemsDisplay** y a su vez la cantidad de numeros que se vizualizaran en la paginacion ej. **< 1 2 3 ... 4 >** con **totalNumbersDisplay**

```javascript
new Interact({
  dbPath: "db.json",
  destFiles: "./dest/",
  pagination: {
    totalItemsDisplay: 4,
    totalNumbersDisplay: 3,
  },
});
```

Crear un nuevo elemento dentro de tu **json** ( "db" )
apuntando hacia un lugar en concreto como en este caso a peliculas con **where** y utilizando data para crear un elemento nuevo ( el ID se genera automaticamente y se va incrementando )

> **NOTA** la estructura de la data seguira la del primer elemento creado, puede colocar por ejemplo categoria primero que nombre pero en este caso pero si el elemento 0 tiene nombre primero, entonces siempre se creara el elemento nuevo asi, esto para mantener una estructura legible dentro del **json**

```javascript
// Create
await Interact.create({
  where: "peliculas",
  data: {
    nombre: "mono",
    categoria: "aventurass",
  }
});
```

Puedes leer la informacion del json sin necesidad de apuntar a un lugar en espeficico pero si lo desea puede apuntar como en este caso a peliculas

```javascript
// Read ---------------
await Interact.read("peliculas");
```

Al actualizar un elemento es necesario tanto como apuntar en donde se realizara el efecto como la **id** y la **data** utilizando **set** seguido de la informacion a actualizar

```javascript
// Update ------------
await Interact.update({
  where: "peliculas",
  id: 3,
  set: {
    nombre: "the raid",
    categoria: "peleas",
  },
});
```

Para eliminar un elemento del **db.json** es tan simple como apuntar hacia donde se realizara la eliminacion y el **id** del elemento a eliminar

```javascript
// Delete -------------
await Interact.delete({
  where: "peliculas",
  id: 3,
});
```

Puede buscar dentro del **db.json** informacion precisa especificando con la propiedad **from** seguido de donde buscara en el elemento en este caso buscara en la propiedad **categoria** todos los elementos que coincidan con aventura

```javascript
await Interact.search({
  where: "peliculas",
  from: {
    categoria: "aventura",
  },
});
```

```javascript
let peliculas = await Interact.read("peliculas");
let results = Interact.pagination({
  items: peliculas,
  currentPage: 2,
});
console.log(results);
```

Puede guardar archivos en una carpeta utilizando **saveFile** que recibe un objeto con propiedades vitales como lo son un array de imagenes que se pasaran a su proyecto ademas de contar con poder ver el progreso actual de carga y el proceso final.

> En **endProcess** se reciben los nombres generados para las imagenes

> Nota: recuerde que tiene que especificar la ruta destino con **destFiles** al momento de instancear **Interact**

```javascript
await Interact.saveFile({
  files: ["./1.mkv", "./1.jpg"],
  checkStatus: (progress) => {
    // 0% - 100%
    console.log(progress);
  },
  endProcess: ( fileNames ) => {
    console.log( fileNames );
  },
});
```
Eliminar archivos con **deleteFile** es realmente sencillo, recibe con **files** todos los elementos a eliminar
```javascript
await Interact.deleteFile({
  files: ["1.jpg", "2.jpg"],
  endProcess: () => {
    console.log("end");
  },
});
```
