# cypress-auto-mock-http-request

[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)

## Copy Notes

This repository is a copy of <https://github.com/scottschafer/cypressautomocker>. We have made this copy for an immediate need to use the library and the correction of specific issues found. We suggest using the original library because these same changes will be suggested to the original library

## How to use

Integrating this tool into your web application involves a few steps:

1. Add the cypress-auto-mock-http-request to your project:

    ```sh
    npm install --save cypress-auto-mock-http-request
    ```

1. Add the cypress web hooks to your application.

    ```js
    import installCypressHooks from 'cypress-auto-mock-http-request/include-in-webapp';
    installCypressHooks();
    ```

    Another option to do the same thing would be to include the following code in your HTML instead:

    ``` html
    <script src="node_modules/cypress-auto-mock-http-request/include-in-webapp/installCypressHooks-norequire.js">
    ```

1. Add the following to cypress/support/commands.js

    ``` js
    import registerAutoMockCommands from 'cypress-auto-mock-http-request/include-in-tests';
    registerAutoMockCommands();
    ```

1. In each of your tests, add the following:

    ``` js
      const MOCK_FILENAME = 'testCounter';

      before(() => {
        cy.automock(MOCK_FILENAME);
      });

      after(() => {
        cy.automockEnd();
      });
    ```

This tool is built on top of the open-source testing platform [Cypress.io](https://www.cypress.io/) to allow recording API results and replaying the APIs as a mock server.

![cypress auto mocker example tests running](https://user-images.githubusercontent.com/1271364/39590019-acdb52ce-4ecd-11e8-94ff-c33cc3894ac7.gif)

##### Folder layout

There are three subfolders within the head directory:
1. example: Contains a simple application to test against that implements APIs with changing responses, along with an example Cypress test.
2. include-in-tests: Contains a library to include in your in your test suite,
3. include-in-webapp: Contains a library to include in your application that is being tested.

##### Running the example

To install and run the example, run the following:
```
cd example
npm install
npm run start
```

You should see `Example app listening at http://localhost:1337` which means that the local server is running on port 1337. It will also open Cypress, allowing you to run the test.

The first time you run the test, our tool will record the API results to the file example/cypress/automocks/testCounter.json. The next time the test is run, it will automatically use the contents of the file to mock the APIs. 

You can control the recording and playback behavior using the (optional) automocker field in cypress.json:
```
  "automocker": {
    "record": true,
    "playback": true
  }
```

The default (that is, if "automocker" doesn't exist) is to treat both record and playback as true, which means that it will automatically record API calls (if the proper commands are called in the tests) if the mock file does not exits, and will play them back as mocks if they do exist.
