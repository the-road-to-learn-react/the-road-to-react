## Deploy to Firebase

After we've built a full-fledged application in React, the final step is deployment. It is the tipping point of getting your ideas into the world, from learning how to code to producing applications. We will use Firebase Hosting for deployment.

Firebase works for React, as well as most libraries and frameworks like Angular and Vue. First, install the Firebase CLI globally to the node modules:

{title="Command Line",lang="text"}
~~~~~~~
npm install -g firebase-tools
~~~~~~~

Using a global installation of the Firebase CLI lets us deploy applications without concern over project dependency. For any globally-installed node package, remember to update it to a newer version with the same command whenever needed:

{title="Command Line",lang="text"}
~~~~~~~
npm install -g firebase-tools
~~~~~~~

If you don't have a Firebase project yet, sign up for a [Firebase account](https://console.firebase.google.com) and create a new project there. Then you can associate the Firebase CLI with the Firebase account (Google account):

{title="Command Line",lang="text"}
~~~~~~~
firebase login
~~~~~~~

A URL will display on the command line that you can open in a browser, or the Firebase CLI opens it. Choose a Google account to create a Firebase project, and give Google the necessary permissions. Return to the command line to verify a successful login. Next, move to the project's folder and execute the following command, which initializes a Firebase project for the Firebase hosting features:

{title="Command Line",lang="text"}
~~~~~~~
firebase init
~~~~~~~

Then choose the Hosting option. If you're interested in using another tool next to Firebase Hosting, add other options:

{title="Command Line",lang="text"}
~~~~~~~
? Which Firebase features do you want to set up for this directory?
 ◯ Firestore: Configure security rules and indexes files for Firestore
 ◯ Functions: Configure a Cloud Functions directory and its files
❯◯ Hosting: Configure files for Firebase Hosting ...
 ◯ Hosting: Set up GitHub Action deploys
 ◯ Storage: Configure a security rules file for Cloud Storage
~~~~~~~

Google becomes aware of all Firebase projects associated with an account after login. However, for this project we will start with a new project on the Firebase platform:

{title="Command Line",lang="text"}
~~~~~~~
First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Please select an option:
  Use an existing project
❯ Create a new project
  Add Firebase to an existing Google Cloud Platform project
  Don't set up a default project
~~~~~~~

There are a few other configuration steps to define. Instead of using the default *public/* folder, we want to use the *dist/* folder from Vite. Alternatively if you set up the bundling with a tool like Webpack yourself, you can choose the appropriate name for the build folder:

{title="Command Line",lang="text"}
~~~~~~~
? What do you want to use as your public directory? dist
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
? Set up automatic builds and deploys with GitHub? No
✔  Wrote dist/index.html
~~~~~~~

The Vite application creates a *dist/* folder after we perform the `npm run build` for the first time. The folder contains all the merged content from the *public/* folder and the *src/* folder. Since it is a single page application, we want to redirect the user to the *index.html* file, so the React router can handle client-side routing.

Now your Firebase initialization is complete. This step created a few configuration files for Firebase Hosting in your project's folder. You can read more about them in [Firebase's documentation](https://bit.ly/3DVgbpG) for configuring redirects, a 404 page, or headers. Finally, deploy your React application with Firebase on the command line:

{title="Command Line",lang="text"}
~~~~~~~
firebase deploy
~~~~~~~

After a successful deployment, you should see a similar output with your project's identifier:

{title="Command Line",lang="text"}
~~~~~~~
Project Console: https://console.firebase.google.com/project/my-react-project-abc123/overview
Hosting URL: https://my-react-project-abc123.firebaseapp.com
~~~~~~~

Visit both pages to observe the results. The first link navigates to your Firebase project's dashboard, where you'll see a new panel for the Firebase Hosting. The second link navigates to your deployed React application.

If you see a blank page for your deployed React application, make sure the `public` key/value pair in the *firebase.json* is set to `dist` (or whichever name you chose for this folder). Second, verify you've run the build script for your React app with `npm run build`. Finally, check out the [official troubleshoot area for deploying Vite applications to Firebase](https://bit.ly/3Sp2Xsn). Try another deployment with `firebase deploy`.

### Exercises:

* Read more about [Firebase Hosting](https://bit.ly/3lXypAC).
  * [Connect your domain to your Firebase deployed application](https://bit.ly/3phFxdp).
* Optional: If you want to have a managed cloud server, check out [DigitalOcean](https://m.do.co/c/fb27c90322f3). It's more work, but it allows more control. [I host all my websites, web applications, and backend APIs there](https://www.robinwieruch.de/deploy-applications-digital-ocean/).
* Optional: [Leave feedback for this section](https://forms.gle/QPjydK8UbaXkxCEj9).