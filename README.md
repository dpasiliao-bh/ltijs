<div align="center">
	<br>
	<br>
	<a href="https://cvmcosta.github.io/ltijs"><img width="360" src="https://raw.githubusercontent.com/Cvmcosta/ltijs/987de79b9a3d529b1b507baa7b7a95d32ab386c2/docs/logo-300.svg?sanitize=true"></img></a>
</div>



> Turn your application into a fully integratable LTI 1.3 tool provider or consumer.


[![travisci](https://img.shields.io/travis/cvmcosta/ltijs.svg)](https://travis-ci.org/Cvmcosta/ltijs)
[![codecov](https://codecov.io/gh/Cvmcosta/ltijs/branch/master/graph/badge.svg)](https://codecov.io/gh/Cvmcosta/ltijs)
[![Node Version](https://img.shields.io/node/v/ltijs.svg)](https://www.npmjs.com/package/ltijs)
[![NPM package](https://img.shields.io/npm/v/ltijs.svg)](https://www.npmjs.com/package/ltijs)
[![NPM downloads](https://img.shields.io/npm/dm/ltijs)](https://www.npmjs.com/package/ltijs)
[![dependencies Status](https://david-dm.org/cvmcosta/ltijs/status.svg)](https://david-dm.org/cvmcosta/ltijs)
[![devDependencies Status](https://david-dm.org/cvmcosta/ltijs/dev-status.svg)](https://david-dm.org/cvmcosta/ltijs?type=dev)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)
[![APACHE2 License](https://img.shields.io/github/license/cvmcosta/ltijs)](LICENSE)
[![Donate](https://img.shields.io/badge/Donate-Buy%20me%20a%20coffe-blue)](https://www.buymeacoffee.com/UL5fBsi)


> v2.4.0
> MAJOR CHANGE
> - Security update, `state` parameter is now validated at the end of the OIDC login flow.
> - Security update, `iss` parameter is now validated at the end of the OIDC login flow.

> - View entire [CHANGELOG](https://cvmcosta.github.io/ltijs/#/changelog)

#### Tested on:

| Version | Moodle | Canvas |
| ---- | - | - |
| 2.4 | <center>✔️</center> | <center></center> |
| 2.3 | <center>✔️</center> | <center>✔️</center> |
| 2.1 | <center>✔️</center> | <center>✔️</center> |
| 2.0 | <center>✔️</center> | <center>✔️</center> |

<sub>**Previous versions are no longer officially supported*</sub>

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Features](#features)
- [Usage](#usage)
- [Documentation](#documentation)
- [Tutorial](#tutorial)
- [Contributing](#contributing)
- [License](#license)


---
## Introduction
According to the [IMS Global Learning Consortium](https://www.imsglobal.org/), the Learning Tools Interoperability (LTI) protocol is an IMS standard for integration of rich learning applications within educational environments. <sup>[ref](https://www.imsglobal.org/spec/lti/v1p3/)</sup>


This package implements a tool provider and consumer (currently in development) as an [Express](https://expressjs.com/) server, with preconfigured routes and methods that manage the [LTI 1.3](https://www.imsglobal.org/spec/lti/v1p3/) protocol for you. Making it fast and simple to create a working learning tool without having to worry about manually implementing any of the security and validation required to do so. 


---


## Installation

### Installing the package

```shell
$ npm install ltijs
```
### MongoDB
- This package uses mongoDB to store and manage the server data, so you need to have it installed, see link bellow for further instructions.
[Installing mongoDB](https://docs.mongodb.com/manual/administration/install-community/)

### PostgreSQL
- This package can also use PosgreSQL to store and manage the server data, it does so through the plugin [ltijs-postgresql](https://www.npmjs.com/package/ltijs-postgresql).


### Firestore
- This package can also use Firestore to store and manage the server data, it does so through the plugin [ltijs-firestore](https://github.com/lucastercas/ltijs-firestore).


---

## Features

| Feature | Implementation | Documentation |
| --------- | - | - |
| Provider | <center>✔️</center> | <center>✔️</center> |
| [Platform Class](https://cvmcosta.github.io/ltijs/#/platform) | <center>✔️</center> | <center>✔️</center> |
| Database plugins | <center>✔️</center> | <center>✔️</center> |
| [Keyset endpoint support](https://cvmcosta.me/ltijs/#/provider?id=keyset-endpoint) | <center>✔️</center> | <center>✔️</center> |
| Grade Service Class | <center>✔️</center> | <center></center> |
| Firebase support | <center>✔️</center> | <center></center> |
| MySql support | <center></center> | <center></center> |
| Redis caching | <center></center> | <center></center> |
| Names and Roles Service Class | <center></center> | <center></center> |




This package implements LTI Provider and Consumer servers. See bellow for specific documentations.

### [LTIjs Provider Documentation](https://cvmcosta.github.io/ltijs/#/provider) 
   - [Platform class documentation](https://cvmcosta.github.io/ltijs/#/platform)

### ~~LTIjs Consumer Documentation~~ (Coming soon)

---

## Usage

### Example of provider usage

> Install LTIJS

```shell
$ npm install ltijs
```

> Install mongoDB

 - [Installing mongoDB](https://docs.mongodb.com/manual/administration/install-community/)


> Instantiate and use Provider class



```javascript
const path = require('path')

// Require Provider 
const LTI = require('ltijs').Provider

// Configure provider
const lti = new LTI('EXAMPLEKEY', 
            { url: 'mongodb://localhost/database' }, 
            { appUrl: '/', loginUrl: '/login', logger: true })


let setup = async () => {
  // Deploy and open connection to the database
  await lti.deploy()

  // Register platform
  let plat = await lti.registerPlatform({ 
    url: 'https://platform.url',
    name: 'Platform Name',
    clientId: 'TOOLCLIENTID',
    authenticationEndpoint: 'https://platform.url/auth',
    accesstokenEndpoint: 'https://platform.url/token',
    authConfig: { method: 'JWK_SET', key: 'https://platform.url/keyset' }
})

  // Set connection callback
  lti.onConnect((connection, request, response) => {
    // Call redirect function
    lti.redirect(response, '/main')
  })

  // Set main endpoint route
  lti.app.get('/main', (req, res) => {
    // Id token
    console.log(res.locals.token)
    res.send('It\'s alive!')
  })
}
setup()
```

---

## Documentation
You can find the project documentation [here](https://cvmcosta.github.io/ltijs).

## Tutorial

You can find a quick tutorial on how to set ltijs up and use it to send grades to a Moodle platform [here](https://medium.com/@cvmcosta2/creating-a-lti-provider-with-ltijs-8b569d94825c).

---

## Contributing

Please ⭐️ the repo, it always helps! 

If you find a bug or think that something is hard to understand feel free to open an issue or contact me on twitter [@cvmcosta](https://twitter.com/cvmcosta), pull requests are also welcome :)


And if you feel like it, you can donate any amount through paypal, it helps a lot.

[![Donate](https://img.shields.io/badge/Donate-Buy%20me%20a%20coffe-blue)](https://www.buymeacoffee.com/UL5fBsi)

### Contributors

<table>
  <tr>
    <td align="center"><a href="https://github.com/Cvmcosta"><img src="https://avatars2.githubusercontent.com/u/13905368?s=460&v=4" width="100px;" alt="Carlos Costa"/><br /><sub><b>Carlos Costa</b></sub></a><br /><a href="#" title="Code">💻</a><a href="#" title="Answering Questions">💬</a> <a href="#" title="Documentation">📖</a> <a href="#" title="Reviewed Pull Requests">👀</a> <a href="#" title="Talks">📢</a></td>
    <td align="center"><a href="https://github.com/lucastercas"><img src="https://avatars1.githubusercontent.com/u/45924589?s=460&v=4" width="100px;" alt="Lucas Terças"/><br /><sub><b>Lucas Terças</b></sub></a><br /><a href="#" title="Documentation">📖</a> <a href="https://github.com/lucastercas/ltijs-firestore" title="Tools">🔧</a></td>
    <td align="center"><a href="https://github.com/micaelgoms"><img src="https://avatars0.githubusercontent.com/u/23768058?s=460&v=4" width="100px;" alt="Micael Gomes"/><br /><sub><b>Micael Gomes</b></sub></a><br /><a href="#" title="Design">🎨</a></td>    
  
  </tr>
  
</table>




---

## License

[![APACHE2 License](https://img.shields.io/github/license/cvmcosta/ltijs)](LICENSE)

