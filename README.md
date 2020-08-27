<h1 align="center">ArcLight</h1>
<p align="center">A Arweave Storage App</p>
<p align="center"><a href="https://arweave.net/-sb8lKu9Vz7BUFshSl_rCyd5sk5feLnxq4f1pzDLHQc">Live Permweb</a> for ArcLight</p>
<p align="center">
  <img src="https://github.com/AyakaLab/ArcLight/workflows/Node%20Build%20Test/badge.svg" />
  <img src="https://github.com/AyakaLab/ArcLight/workflows/Production%20CI%20Build%20Test/badge.svg">
</p>

### Interact with Arweave
```
yarn dev
```

### Deploy using a key
```
arweave deploy-dir ./dist --key-file ./secrets/key.json
```

### Reference
[arweave interface](https://www.arweave.org/build)
[Arweave.js](https://github.com/ArweaveTeam/arweave-js): A library for interacting with the Arweave network from web applications and node.js programs.
[Arweave Deploy](https://github.com/ArweaveTeam/arweave-deploy): A simple command line tool for deploying web apps, pages, and other files to the permaweb.
## Build Setup

``` bash
# install dependencies
yarn

# serve with hot reload at localhost:8080
yarn dev

# build for production with minification
yarn build

# build for production and view the bundle analyzer report
yarn build --report

# run unit tests
yarn unit

# run all tests
yarn test
```

