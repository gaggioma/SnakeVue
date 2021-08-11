# Snake :snake: with Vue.js 

## Project target :dart:
The main purpose of this project is to try the front end library [Vue.js](https://vuejs.org/v2/guide/) in a dynamic web application.
I thouth that a simple example that best fit this purpose colud be the old [Snake](https://en.wikipedia.org/wiki/Snake_(video_game_genre)) video game.

## Project structure
Components:
- Arena: this component play the role of HOC. Arena controls all positions of snake, food and executes bounding check;

- CountDown: this component executes countdown before game start;

- Food: component which represent food in the arena;

- Snake: representation of snake singl component. The full queue is genetated by Arena like an array.  

## Result :video_game: 


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```