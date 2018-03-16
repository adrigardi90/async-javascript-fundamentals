## In this demo we are goin to polay with the user interactions.
* References: https://developer.mozilla.org/es/docs/Web/API/DataTransfer
https://codepen.io/patrickhlauke/pen/azbYWZ

* Folder structure:

05 User interactions/
├── src/
│   ├── content/
|   |   ├── site.css
│   ├── js/
|   |   ├── main.js
│   ├── index.html
│   └── bootstrap-theme.min.css
├── gulpfile.js
├── package.json

## Steps.

### 1. Create the `index.html` for our demo.

* Just paste this code.

```html
<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8">
  <title>LEMONCODE 16/17 jQuery</title>
  <link rel="stylesheet" href="content/site.css">
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
</head>

<body>
  <h1>API Rest</h1>
  <div id="drag-element" draggable="true" class="drag"></div>
  <br>
  <div id="drop-element-a" class="drop"></div>
  <div id="drop-element-b" class="drop"></div>
  <script src="./js/main.js"></script>
</body>
</html>

```

### 2. Create `site.css`

* Just paste this code,

```css
@import url('https://fonts.googleapis.com/css?family=Raleway');

body {
  font-family: 'Raleway', sans-serif;
}

.container {
  padding: 2em;
}

.container p {
  max-width: 70%;
  font-size: 0.9em;
  margin-top: 0.2em;
  margin-bottom: 0.2em;
  height: 2.3em;
}

.container p img {
  max-width: 95%;
  max-height: 95%;
  position: relative;
  top: 0.7em;
  left: 1em;
}

.container ul {
  list-style: none;
}

.drag {
  background-color: cyan;
  width: 200px;
  height: 200px;
}

.drop {
  border: 1px solid black;
  width: 300px;
  height: 300px;
  float: left;
}

```
* Run the application and have a look

### 3. Lets type the javascript code to manage the drag and drop

```javascript
document.onreadystatechange = () => {
  if(document.readyState === 'complete') {

    const dragStart = (event) => {
      event.dataTransfer.setData('text/plain', event.target.id);
      console.log("dragStart", event);
    };

    const dragElement = document.getElementById('drag-element');
    dragElement.addEventListener('dragstart', dragStart);

    const dragOver = (event) => {
      event.preventDefault();
      event.dataTransfer.dropEffect = 'move';
      console.log({
        x: event.pageX,
        y: event.pageY
      });
    };

    const drop = (event) => {
      const id = event.dataTransfer.getData('text');
      console.log('drop', id);
      event.target.appendChild(dragElement);
    };

    const dropElementA = document.getElementById('drop-element-a');
    const dropElementB = document.getElementById('drop-element-b');

    dropElementA.addEventListener('dragover', dragOver);
    dropElementB.addEventListener('dragover', dragOver);

    dropElementA.addEventListener('drop', drop);
    dropElementB.addEventListener('drop', drop);
  }
};

```
* Drag and drop it's a great example of async sources, code that it's executed later.
* Obviously these are user interactions, that it's getting the elements and moving around the page.
* When we click the drag element and start the dragging around, the `dragstart` event it's triggered. The associated handler will be pushed to the queue, and later on executed.
* How many little programs do we have here?
## In this demo we are going to play with the user interactions.
* Folder structure:

05 User interactions/
├── src/
│   ├── content/
|   |   ├── site.css
│   ├── js/
|   |   ├── main.js
│   ├── index.html
│   └── bootstrap-theme.min.css
├── gulpfile.js
├── package.json

* Start from previous code demo.

## Steps.

### 1. Refactor the `index.html` for our demo.

* Just paste this code.

```diff html
<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8">
  <title>LEMONCODE 16/17 jQuery</title>
  <link rel="stylesheet" href="content/site.css">
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
+  <script src="./js/main.js"></script>
</head>

<body>
  <h1>API Rest</h1>
+  <div id="content"></div>
-  <div id="drag-element" draggable="true" class="drag"></div>
-  <br>
-  <div id="drop-element-a" class="drop"></div>
-  <div id="drop-element-b" class="drop"></div>
-  <script src="./js/main.js"></script>
</body>
</html>

```

### 2. Refactor `site.css`

* Just paste this code,

```diff css
@import url('https://fonts.googleapis.com/css?family=Raleway');

body {
  font-family: 'Raleway', sans-serif;
}

.container {
  padding: 2em;
}

.container p {
  max-width: 70%;
  font-size: 0.9em;
  margin-top: 0.2em;
  margin-bottom: 0.2em;
  height: 2.3em;
}

.container p img {
  max-width: 95%;
  max-height: 95%;
  position: relative;
  top: 0.7em;
  left: 1em;
}

.container ul {
  list-style: none;
}

-.drag {
-  background-color: cyan;
-  width: 200px;
-  height: 200px;
-}
-
-.drop {
-  border: 1px solid black;
-  width: 300px;
-  height: 300px;
-  float: left;
-}
-
```

### 3. Remove the previous code and place the start point.

```javascript
document.getElementById('content').innerHTML = 'Main content from JS';

```
* Run the application. No text appears on the browser's window. Why?
    * The reason it's that the document does not load yet, so the element with `id = content`, does not exist as well.
* Of course, we have this behavior because, the script it's loaded on the head.

### 4. To fix this behavior, we are going to use a delay.

```diff main.js
-document.getElementById('content').innerHTML = 'Main content from JS';
+setTimeout(() => {
+    document.getElementById('content').innerHTML = 'Main content from JS';
+}, 100);
```
* En la función de arriba, cuando pasen 100 ms, esa función que estamos suministrando aquí será empujada a la cola del event loop, y una vez que se ejecute, lo que sea que estuvierá por delante se ejecuté, entonces después esta, función comnzará su ejecución. Esto, nos muestra un aspecto importante de esta función, no hay garatía sea llamada exactamente a los 100ms, cuando es alcanzada. A los 100ms, será empujada a  la cola, pero si hay mucho trabajo, en frente, tendrá que esperar a que ese trabajo, se ejecute antes. 

* Analogía con un dietario.