# axios-digest-auth

## Overview

A library which implements HTTP digest authentication in a manner which should be familiar to any 
project which also uses Axios. Can be used in both Node.js and browser environments.

This library is not affiliated with the Axios project or its maintainers, other than its input/output data structures being identical to those Axios uses, and it relying on Axios.

This is fork of [@mhoc/axios-digest-auth](https://github.com/mhoc/axios-digest-auth) package

This improved version of the package adds support for the following:
* returns 'opaque' value from the server as part of the response headers (see [RFC 7616](https://httpwg.org/specs/rfc7616.html))


### Installation

```
$ npm i @acidemic/axios-digest-auth
```

### Essential Usage

```js
import AxiosDigestAuth from '@acidemic/axios-digest-auth';

const digestAuth = new AxiosDigestAuth({
  username: "myDigestUsername",
  password: "myDigestPassword",
});

const MakeARequest = async () => {
  const response = await digestAuth.request({
    headers: { Accept: "application/json" },
    method: "GET",
    url: "https://cloud.mongodb.com/api/atlas/v1.0/groups",
  });
}
```


## API

### class AxiosDigestAuth

```js
import AxiosDigestAuth from "@acidemic/axios-digest-auth";
```

Application-facing class which stores username/password state and executes requests.


### new AxiosDigestAuth()

```js
const options: AxiosDigestAuthOpts = {
  axios,
  password,
  username,
};
const axiosDigestAuthInst = new AxiosDigestAuth(options);
```

* => **AxiosDigestAuthOpts**: input object (see below)


### request()

```js
import * as axios from "axios";
// you dont have to import this or add it to your package.json.
// just using it to outline where these types are coming from.

const ExecuteMyRequest = async () => {
  const requestOptions: axios.AxiosRequestConfig = {
    headers: { Accept: "application/json" },
    method: "GET",
    url: "https://cloud.mongodb.com/api/atlas/v1.0/groups",
  };
  const response: axios.AxiosResponse = await axiosDigestAuthInst.request(requestOptions);
};
```

Executes a request. The signature of this function is identical to Axios's own request method.

* => **axios.AxiosRequestConfig**: refer to the [Axios documentation](https://github.com/axios/axios#request-config) for more information on this type.
* <= **axios.AxiosResponse**: refer to the [Axios documentation](https://github.com/axios/axios#response-schema) for more information on this type.


### interface AxiosDigestAuthOpts

```js
import { AxiosDigestAuthOpts } from "@acidemic/axios-digest-auth";
```

* **axios (axios.Axios | undefined)**: Optionally provide an axios object with which requests are made. If this is not provided, axios-digest-auth will create one for you by simply using the axios library default export, with no configuration.
* **password (string)**: the HTTP digest authentication password to use.
* **username (string)**: the HTTP digest authentication username to use.


Check out [the documentation site](https://axios-digest-auth.mhoc.co) for more information 
on usage.


## Usage in the Browser with Webpack and Typescript

This module utilizes certain built-in Node.js modules. To use it in a browser, you'll need to include a polyfill for each of these modules.

Firstly, install all necessary modules by executing the following command in your terminal:

```sh
npm install crypto-browserify buffer stream-browserify url
```

Next, add a fallback in your webpack configuration file (typically named webpack.config.js):

```js
module.exports = {
  //...
  resolve: {
    fallback: {
      "crypto": require.resolve("crypto-browserify"),
      "buffer": require.resolve("buffer/"),
      "stream": require.resolve("stream-browserify"),
      "url": require.resolve("url/")
    },
  },
};
```
