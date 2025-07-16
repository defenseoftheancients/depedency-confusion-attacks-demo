## What is Dependency confusion?
Dependency confusion (also known as dependency repository hijacking, substitution attack, or repo jacking for short) is a software supply chain attack that substitutes malicious third-party code for a legitimate internal software dependency.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4fcab08a-ad28-4ff7-bf83-203efa494f84" />

## Sections
> [!NOTE]
> ⚠️ This demo only works if your private registry has NOT disabled fallback to the public npm registry. If your private registry is configured correctly (i.e. fallback to the public registry is disabled), then the public malicious package will not be installed, and the attack will fail.

### 🧪 Technology
* Programming Language: Javascript
* Public registry: npmjs - [Download & Install](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* Private registry: Any JavaScript-compatible registry like:
  
  * AWS CodeArtifact
  * Verdaccio
  * GitHub Packages
  * GitLab Packages
  * ...
    
### 📁 Directory Structure
```
├── dependency-conconfusion-private-package 
├── dependency-conconfusion-private-package
├── index.js
└── packackge.js
```

* `dependency-conconfusion-private-package`: The private package
* `dependency-conconfusion-public-package`: The public attacker-controlled package
* `index.js`: Main file to execute
* `package.js`: Declares dependency on the package
> [!NOTE]
> ⚠️ The scope of the package is set to @your-scope. Please change it to match your registry or test case, e.g. "name": "@aws/sum-function".


### 🚀 Steps to Run the Demo
1. Cloning the repository
* Open a Terminal and type (or copy / paste) the command below in, then press Enter
```sh
git clone https://github.com/defenseoftheancients/dependency-confusion-demo.git
cd dependency-confusion-demo
```

2. Publish the private package to your private registry
```sh
cd dependency-confusion-private-package
npm login --registry=https://your-private-registry/
npm publish --access=restricted
```
   
3. Publish the same package name to the public registry
```sh
cd ../dependency-confusion-public-package
npm login   # for npmjs
npm publish --access=public
```

4. Run the demo (simulate dependency installation)
```sh
cd ..
npm install
node index.js
```

If dependency confusion is successful, code from the public package will be executed instead of the private one.
<hr>
🎯 This project demonstrates how dependency confusion occurs when:

* A project depends on a private scoped package
* But that package is accidentally resolved from the public npm registry due to misconfigured registry settings or caching
