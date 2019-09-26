# sheriff-of-nottingham
## What is this repo?
One of my favorite board games is Sheriff of Nottingham. It's unfortunate that I'm terrible at determining whether other players are lying! If only there was a way to tell if they were truthfully declaring their goods...

This small web app is designed to count cards automatically. I'm creating this as an experiment; I want to see if counting cards is an effective means by which the Sheriff can tell if a player is lying.

## What is Sheriff of Nottingham?
It's a board game for 3-5 players. Here is a link to a video that describes the rules: (https://www.youtube.com/watch?v=ofZS29XArzM)[https://www.youtube.com/watch?v=ofZS29XArzM]

## How does this app work?
This app is built around the idea that a player is lying if and only if their merchant bag does not contain the goods they claim it contains. For example, a player may declare that their bag contains "three apples" when it contains two apples and one bread. In this case, the player is lying.

Since a player can only place cards from their hand into their bag, I believe the Sheriff can determine whether or not a player is lying based solely on what cards are in play in any given round. This app uses elementary probabilities to estimate what cards are in play.

## Technical Stuff
This project uses VueJS. I wanted to take advantage of Vue's computed values.

### Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
