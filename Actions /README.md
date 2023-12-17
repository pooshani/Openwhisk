# OpenWhisk Actions
## how to create an invoke a Function
### 1- Create a file and Save the code to a file. For example, hello.js

    function main(args) {
      const name = args && args.name || "stranger";
      return { greeting: `Hello ${name}!` };
      }
### 2- From the OpenWhisk CLI command line, create the action by entering this command:

    wsk -i action create helloJS hello.js
### 3- Invoke the action by entering the following commands:

    wsk action invoke helloJS --result --param name World

## Web Actions
Web actions are OpenWhisk actions annotated to quickly enable you to build web based applications. This allows you to program backend logic which your web application can access anonymously without requiring an OpenWhisk authentication key. It is up to the action developer to implement their own desired authentication and authorization.

Let's take the following JavaScript action hello.js:

    function main({name}) {
      var msg = 'you did not tell me who you are.';
      if (name) {
        msg = `hello ${name}!`
      }
      return {body: `<html><body><h3>${msg}</h3></body></html>`}
    }
You may create a web action hello in the package demo for the namespace guest using the CLI's --web flag with a value of true or yes:

    wsk package create demo
    ok: created package demo
    
    wsk action create /guest/demo/hello hello.js --web true
    ok: created action /guest/demo/hello
    
    wsk action get /guest/demo/hello --url
    ok: got action hello
    https://${APIHOST}/api/v1/web/guest/demo/hello
    
