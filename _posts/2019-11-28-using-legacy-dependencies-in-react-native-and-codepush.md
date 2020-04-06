---
title: "Using legacy dependencies with React Native and CodePush"
date: "2019-11-28"
---

Growing a React Native app over time can be a complex undertaking. Dependencies constantly evolve, with breaking changes to iOS/Android-specific code.

![](images/code_quality.png)

Source: XKCD

We deploy our apps with [CodePush](https://microsoft.github.io/code-push/) for super-fast iterations omitting the slow middleman - the App/Play Store - most of the time.

That approach can have its drawbacks - at some point you will have to support users that are running older versions of your app, but will want to have the latest features/bug fixes aka CodePush build. What if you wanted to upgrade a dependency that had breaking changes to its native code? **How would you do that maintaining a single codebase and CodePush deployment?**

To make this easier to visualize, let's say we're using the package `[react-native-branch](https://www.npmjs.com/package/react-native-branch)` in our project, and we are on version `3.1.1` of it. We want to update to the newest release `4.2.1` (at the time of writing), but it has breaking changes to the iOS/Android code. In order to do that, we can't simply update the version number in our `package.json`, as this will cause conflicts in our app; in order to do this smoothly, supporting older versions of our app and the newest, this is what we'll have to do:

1. Fork the package.  
      
    As we cannot reference the same package with different versions in our `package.json`, we'll have to create a forked package from the currently installed package.  
      
    So, before we update our `react-native-branch` package in our `package.json` we do the following:  
      
    `$ cp -r node_modules/react-native-branch ~/Projects/react-native-branch-3.1.1`  
      
    We effectively copied the currently installed package. We now `cd` into that project and initialize a git repo.  
    
2. Upload to GitHub  
      
    Once we "forked" the old package, we upload it to GitHub - I recommend naming it `react-native-branch-3.1.1` - that way it's clearly identifiable.  
      
    Before we upload the package, we want to do one change to the project's `package.json`:  
      
    `- "name": "react-native-branch",  
    + "name": "react-native-branch-3.1.1",`  
      
    We do this to avoid namespace clashes when we install the latest version of the package in our React Native project. You'll see specifically why, later if you read on.  
    
3. Reference the forked package, and install the new package.  
      
    `"react-native-branch": "4.2.1",  
    "react-native-branch-3.1.1": "https://github.com/kennym/react-native-branch-3.1.1",`  
      
    Run  
      
    `$ yarn install`  
      
    ...aaaand, we're almost done.

We have two more things to solve.  
  
**The first is,** when we build our app with XCode or Gradle, React Native will want to use both packages, but we really only care about the latest package in our shell, and in order to tell that to React Native, we'll have to create a `react-native.config.js` in our project's root directory.

```
module.exports = {
  assets: [],
  dependencies: {
    'react-native-branch-3.1.1': {
      platforms: {
        ios: null,
        android: null,
      },
    },
  },
};
```

Done. This tells React Native to ignore the previous react-native-branch version during compile time, and use the new package, as the Native API changed.

The **second thing** we have to do is bump the version of our React Native app, so we have a reference of when to use the old and new library.

_If you don't have a system for that yet, I highly recommend: [https://www.npmjs.com/package/react-native-version-up](https://www.npmjs.com/package/react-native-version-up)_

![](images/the_general_problem.png)

Source: XKCD

Once that is done, we can do the following in our application code:

```
let branch
if (semver.satisfies(VersionNumber.appVersion, '>=1.0.1')) {
  // Use the latest react-native-branch
  branch = require('react-native-branch').default
} else {
  // Use our forked package
  branch = require('react-native-branch-3.1.1).default // This is where the package.json name change comes in
}
```

Now we should be able to deploy to CodePush and be running the same code both on old and new version of our app.

## Advantages

- The great thing about this approach is that it also supports packages with obfuscated source code, from closed-source third parties, as long as they provide an NPM package.

## Disadvantages

- We noticed an increase in the size of or CodePush JavaScript bundle, since we're including multiple versions of the same library.
- Dealing with legacy dependencies in your codebase, instead of forgetting about them. (I assume that's what you do, when you deploy via the store only)

* * *

If you find this helpful, and you're a parent you might want to check out the Parenting App we're building at [www.getbright.com](http://www.getbright.com).
