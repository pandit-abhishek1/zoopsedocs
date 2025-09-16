## Backend Setup

#### System Requirement
1. Nodejs versions: 
2. Docker  versions: 
3. Git      versions: 
#### Installations
##### 1. Repository clone
open your terminal and use this command to clone the repo
```bash 
git clone https://github.com/zoopseindia/CRM_backend.git
```
##### 2. NPM package cloning
this step clone all the requiremnets packages in your project and make  `node_modules`  folder putting all the packages.
 * `Remember` Go to Services one by one and run this command.
 * There is no need to run this commond in `Shared Code`.
```bash
 npm i 
```
* Run script seeing the package.json for respective task. e.g. for running dev mode
```bash
  npm run dev
```

##### 3. Docker Services Setup
this section you make containers for services used in the project.
* Go to the root of docker compose file and run this script in your terminal.
```bash
    docker compose up -d
```
* remember you can run particular services on your need using this command.
```bash
   docker compose up -d servics_name1 services_mname2
```


### Cool
Now You are Setup the Project in Your local machine. keep contributing.
  
  
  


