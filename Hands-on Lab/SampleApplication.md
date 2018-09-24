## Starter app

Once the VM is deployed, connect to it using an RDP client, fork the starter application into your own GitHub repo, and clone it to the Lab VM.

The starter project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

In the project directory, run:

```sh
npm install
node data/Seed.js
npm run build
npm start
```

### `npm install`

Installs required components.

### `node data/Seed.js`

Seeds the local MondoDB database with sample data.

### `npm run build`

Builds the app for production to the `build` folder. It correctly bundles React in production mode and optimizes the build for the best performance. The build is minified and the filenames include the hashes.

Your app is ready to be deployed!

### `npm start`

Runs the app in the development mode. You will also see any lint errors in the console.

Open [http://localhost:3000](http://localhost:3000) to view it in the browser.