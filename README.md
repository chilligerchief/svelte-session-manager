[![Build Status](https://secure.travis-ci.org/arlac77/svelte-session-manager.png)](http://travis-ci.org/arlac77/svelte-session-manager)
[![codecov.io](http://codecov.io/github/arlac77/svelte-session-manager/coverage.svg?branch=master)](http://codecov.io/github/arlac77/svelte-session-manager?branch=master)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![Coverage Status](https://coveralls.io/repos/arlac77/svelte-session-manager/badge.svg)](https://coveralls.io/r/arlac77/svelte-session-manager)
[![Dependency Status](https://david-dm.org/arlac77/svelte-session-manager.svg)](https://david-dm.org/arlac77/svelte-session-manager)
[![devDependency Status](https://david-dm.org/arlac77/svelte-session-manager/dev-status.svg)](https://david-dm.org/arlac77/svelte-session-manager#info=devDependencies)
[![docs](http://inch-ci.org/github/arlac77/svelte-session-manager.svg?branch=master)](http://inch-ci.org/github/arlac77/svelte-session-manager)
[![downloads](http://img.shields.io/npm/dm/svelte-session-manager.svg?style=flat-square)](https://npmjs.org/package/svelte-session-manager)
[![GitHub Issues](https://img.shields.io/github/issues/arlac77/svelte-session-manager.svg?style=flat-square)](https://github.com/arlac77/svelte-session-manager/issues)
[![Greenkeeper](https://badges.greenkeeper.io/arlac77/svelte-session-manager.svg)](https://greenkeeper.io/)
[![Known Vulnerabilities](https://snyk.io/test/github/arlac77/svelte-session-manager/badge.svg)](https://snyk.io/test/github/arlac77/svelte-session-manager)
[![minified size](https://badgen.net/bundlephobia/min/svelte-session-manager)](https://bundlephobia.com/result?p=svelte-session-manager)
[![npm](https://img.shields.io/npm/v/svelte-session-manager.svg)](https://www.npmjs.com/package/svelte-session-manager)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/arlac77/svelte-session-manager)
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier)

# svelte-session-manager

Session store for svelte (currently only for JWT)

# usage

```js
import { derived } from 'svelte';
import { Session, login } from 'svelte-session-manager';

// use localStorage as backng store
let session = new Session(localStorage);

// session may still be valid
if(!session.isValid) {
  await login(session, 'https://mydomain.com/authenticate', 'a user', 'a secret');
}

session.isValid; // true when auth was ok


export const values = derived(
  session,
  ($session, set) => {
    if (!session.isValid) {
      set([]); // session has expired no more data
    } else {
      fetch('https://mydomain.com/values', {
        headers: {
          ...session.authorizationHeader
        }
      }).then(async data => set(await data.json()));
    }
    return () => {};
  }
);

// $values contains fetch result as long as session has not expired
```

## run tests

```sh
export BROWSER=safari|chrome|...
npm|yarn test
```

# API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [SessionData](#sessiondata)
    -   [Properties](#properties)
-   [Session](#session)
    -   [Parameters](#parameters)
    -   [Properties](#properties-1)
    -   [save](#save)
    -   [authorizationHeader](#authorizationheader)
    -   [isValid](#isvalid)
    -   [invalidate](#invalidate)
    -   [subscribe](#subscribe)
        -   [Parameters](#parameters-1)
-   [login](#login)
    -   [Parameters](#parameters-2)

## SessionData

Data as preserved in the backing store

Type: [Object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)

### Properties

-   `username` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** user name (id)
-   `access_token` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** JWT token

## Session

User session
To create as session backed by browser local storage

```js
let session = new Session(localStorage);
```

### Parameters

-   `data` **[SessionData](#sessiondata)** 

### Properties

-   `entitlements` **[Set](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Set)&lt;[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** 
-   `expirationDate` **[Date](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Date)** 
-   `store` **[SessionData](#sessiondata)** backing store to use for save same as data param

### save

persist into the packing store

### authorizationHeader

http header suitable for fetch

### isValid

As long as the expirationTimer is running we must be valid

Returns **[boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** true if session is valid (not expired)

### invalidate

remove all tokens from the session and the backing store

### subscribe

Fired when the session changes

#### Parameters

-   `subscription` **[Function](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function)** 

## login

Bring session into the valid state by callinf the authorization endpoint
and asking for a access_token

### Parameters

-   `session` **[Session](#session)** to be opened
-   `endpoint` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** authorization url
-   `username` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** id of the user
-   `password` **[string](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** user credentials

# install

With [npm](http://npmjs.org) do:

```shell
npm install svelte-session-manager
```

# license

BSD-2-Clause
