# jigsolver
A drag-and-drop jigsaw puzzle solver app in Vue.js

#### setup:
1. clone the repo

`git clone https://github.com/JCY-Home/jigsolver.git jigsolver`

2. install dependencies

```
cd jigsolver/
sudo npm install
```

3. for development:

`npm run dev`

this will serve the page at **localhost:8080** by default.
all assets for dev live in the `./src/` folder.

for deployment:

`npm run build`

#### implementation:

Puzzle board is based on a 900x900-px image, divided into 9 equal pieces of 300x300-px each. Image pieces live in ./static/tr-pieces, and must be named "piece1.png, piece2.png, etc" (for now)

Drag-and-drop functionality uses only Vue, the HTML5 DnD API, and vanilla JS (no jQuery)


