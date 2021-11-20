# Mr Bucket

<p align="center">
  <a href="https://www.youtube.com/watch?v=D_Bbf7hX5I0">
    <img src="https://img.youtube.com/vi/D_Bbf7hX5I0/0.jpg"/>
  </a>
</p>

Largely taken from [this Vue.js Websocket Tutorial on TutorialEdge.net](https://tutorialedge.net/javascript/vuejs/vuejs-websocket-tutorial/). Except for a few key differences:

- messages are displayed on the page.
- a valid test endpoint is provided if the user does not have one (thanks to [Pie Socket](https://www.piesocket.com/websocket-tester)).
- ONE-WAY communication only. Two way communication will NEVER be implemented (although I welcome PRs or anyone to fork)

This project was mainly done in part because I just needed something to connect to a websocket and then display messages being sent by another source application.

# TODO

Following is a non-comprehensive list of things that I want to do, barring time and...drive/will. You know how proof-of-concepts/hacks/toy projects are 🥲

- [ ] allow for customized `on<event>` payloads/messages (e.g.: Rails ActionCable subscriptions on WS connection/`onopen`)
- [ ] better event display (right now just a nasty `pre` dump); JSON pretty-print, collapse + colors? 🤷‍♂️ 
- [ ] message filter (e.g.: omit `ping` requests)
- [ ] messages history re-render throttling for things like...I don't know, load testing? Probably not the right tool but might be fun 🤷‍♂️
- [ ] Dockerized image + auto-initialization/startup with an endpoint to connect to
- [ ] message timeline + d3 graph? that would be _really_ fun

Refactors may or may not happen (forgive my messy code); this _is_ supposed to just be a toy project, after all.

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

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## Usage

Enter a Websocket URL that you want to listen to. To test you can just use a URL from [Pie Socket](https://www.piesocket.com/websocket-tester)
