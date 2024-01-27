# Env Public

## Purpose
This application was put built to make sharing of a public env file possible with the ability to generate a local .env from a shared .envpub file. It was also built to make local development easier with a wrapper command for nodemon and any script, it will decrypted variables injected after password prompt.

## Installation
This application should be install globally but could possibly be run locally if needed. This has only currently been testing in nodejs 16 but should work for anything newer and may work for older versions.
```
npm install -g envpub
```

## Features
This application allows for variables to be stored in the following format.

- unencrypted, private, name shared (json format)
- encrypted and shared, name shared (json format)
- encrypted and private, name shared (json format)
- unencrypted, private, name not shared (plain text and only in .env file)

The application allows you to run a web application using nodemon, the below app.js is an express application in your current application and password will only be prompted when starting the application.
```
envpub web app.js
```

The application allows you to run a node script or any script with decrypted environment variables.
Be aware if require optional parameters for your script .e.g --test then you will need to add -- as an argument before it
```
envpub app node app.js parameter1 parameter2
envpub app node app.js parameter1 -- --optional test
```
```
envpub app app.sh
```

## Getting started
- Install the envpub application as shown above.
- run 
    ```envpub init``` on the command line in your application directory.
- if you already have a .envpub it will generate your .env from it, if not it will generate one from scratch.

## How it works
The application works by managing two files, a .env file which should be git ignored and .envpub which you would want to commit with your repository. the only difference between the two is that the private name shared variables values will not be saved in the .envpub file, it will be saved as an empty string.
The algorythm used for the encryption is ```aes-192-cbc```.

Example .env file
```
MY_SHARED_ENCRYPTED={"encrypted":true,"private":false,"value":"375b12b3ff40caae28e450b78f98480d","iv":"e6f270264efedfcc8d644168c22e79e7","salt":"4a16509b-e99d-4b3a-b5b8-a49f153b22dd"}
MY_SHARED_PRIVATE={"encrypted":false,"private":true,"value":"Testing Testing 123"}
MY_SHARED_ENCRYPTED_PRIVATE={"encrypted":true,"private":true,"value":"375b12b3ff40caae28e450b78f98480d","iv":"e6f270264efedfcc8d644168c22e79e7","salt":"4a16509b-e99d-4b3a-b5b8-a49f153b22dd"}
MY_PRIVATE_ONLY=Only stored in env file
```

Example .envpub file
```
MY_SHARED_ENCRYPTED={"encrypted":true,"private":false,"value":"375b12b3ff40caae28e450b78f98480d","iv":"e6f270264efedfcc8d644168c22e79e7","salt":"4a16509b-e99d-4b3a-b5b8-a49f153b22dd"}
MY_SHARED_PRIVATE={"encrypted":false,"private":true,"value":""}
MY_SHARED_ENCRYPTED_PRIVATE={"encrypted":true,"private":true,"value":"","iv":"e6f270264efedfcc8d644168c22e79e7","salt":"4a16509b-e99d-4b3a-b5b8-a49f153b22dd"}
```

As you can see from above anything that is set as private will have a blank value in the .envpub file.
The adding, updating and deleting of variables is all managed by the envpub command line application, you will be prompted for what to enter by the application. if you are not sharing the name you can just add plain text variables to the .env file manually.
```
envpub add 
envpub update
envpub delete
```

Also be aware that there is only a single password for the entire file if using encryption, this was done for simplicity.
You can change a password by running the following command
```
envpub change-password
```
You can also performa a json dump with encrypted data in file by running the following command
```
envpub dump-data
```

It is also possible to encrypt and decrypt string using the following commands, this is useful if you have had two people enter different passwords in the shared file and data has been merged as this would require you  to extract the data out manually, coordination between team members is manual for the shared password.
```
envpub encrypt <string>
envpub decrypt <string>
```

You should also backup your data in keepass or a similar application in case of data loss.