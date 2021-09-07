# Introduction 
This is an example solution of a SharePoint MVC implementation using PNPjs, Spfx-Controls and Fluent UI.
It hopefully illustrates:
- create a model for the SharePoint access
- reusable modules for the basics, data source access and presentation
- focus on business logic development
- an extendable framework by YOUR contribution, every contribution counts

# Table of contents
1. [Getting Started](#getting-started)
    1. [Requirements](#requirements)
    2. [Minimal path to awesomeness](#minimal-path-to-awesomeness)
3. [Build and install](#build-and-install)
4. [Contribute](#contribute)
    1. [To do list](#to-do-list)
5. [Create new project](#create-new-project)

# Getting Started
You can use the "Minimal path to awesomeness" to have look around. Please brnach before contributing.
You can follow the "Create a new project" to start with a copy of this a starting point for a new project (usually app)

## Requirements
You should have the following installed:
- Visual Studio Code (or similar)
- Node 14
- yarn
- git
- GitExtension (the program, if your contributing or forking a new project of this)

## Minimal path to awesomeness
```
yarn global add lerna
git clone --recurse-submodules https://github.com/mauriora/WebPart-Example-Solution.git
cd WebPart-Example-Solution
lerna bootstrap
code .
```
open a terminal in the solution (root) folder
The `node_modules` folder in the solution should contain the majority of dependencies, while the `node_modules` folder of each sub modules should only contain very few dependencies if any.
All modules in `shared` should be linked in the solutions `node_modules` folder.

```
yarn build-shared
yarn serve
```
[browse to the workbench on *YOUR-SITE*](https://YOUR-DOMAIN.sharepoint.com/sites/YOUR-SITE/_layouts/15/workbench.aspx)
add the webpart to the page

# Build and install
1. In a solution terminal execute `yarn workspace @mauriora/webpart-example serve`
2. [browse to the sharepoint app store on *YOUR-TENANT*](https://YOUR-TENANT.sharepoint.com/sites/apps/AppCatalog/Forms/AllItems.aspx)
3. Click **Upload**
4. Click **Choose files**
5. Navigate to *YOUR-SOLUTION*/apps/*YOUR-PROJECT*/sharepoint/solution
6. Select *YOUR-PROJECT*.sppkg and click **Open**
7. Add a comment and click **OK**
8. Wait for the upload to finish and a dialog to open
9. If you want the webpart to be available on all sites without installing, tick **Make this solution available to all sites in the organization**, otherwise it needs to be installed on each site using
10. Click **Deploy**
11. grant API permissions in [Sharepoint Admin Center: Advanced / API access](https://YOUR-DOMAIN-admin.sharepoint.com/_layouts/15/online/AdminHome.aspx#/webApiPermissionManagement)
12. If not installed tenant wide, go to each site you wnat the webpart on
    1. Go to *Site Content*
    2. Click **+ New** -> **App**
    3. Find *YOUR-APP* and click on it
    4. Wait until it's installed
13. Create a page and add **YOUR-WEBPART**

# Contribute
Use the minmal path to awesomeness and please create a branch for your contribution.
Then do a pull request to merge your branch into the main branch.

## To do list

### Documentation
1. [ ] Architecture overview


### Framkework
1. [ ] Replace lerna with rushstack
2. [ ] Create Azure build pipeline
    1. [ ] .sppkg become artifacts
    2. [ ] sub modules npm packages become artifacts

### Implementation
1. [ ] Styling (js based)
2. Controller
    1. [ ] Single Column Taxonomy fix through TaxCatchAll field

### Modules
1. [ ] Utils module
1. [ ] Theming/branding module per client


# Create new project
- Fork the solution as *YOUR-SOLUTION*
- Clone the forked solution
- Fork the starting app project as *YOUR-PROJECT*
- Add the *YOUR-PROJECT* as submodule to *YOUR-SOLUTION* using the app GitExtenson, manage sub modules
- Remove the starting project, and any other you don't need, from YOUR-solution using the app GitExtenson, manage sub modules
- call `yarn initguids` in apps/*YOUR-PROJECT*
- rename *YOUR-.....* in:
    - app/*YOUR-PROJECT*
        - packgae.json
        - src/webparts/*YOUR-WEBPART*
            - *YOUR-WEBPART*.manifest.json
        - config
            - config.json
            - package-solution.json

# Add a new sub module to the solution
1. Create a new repository with at least one file (e.g. default README.md)
2. Copy the clone URL to the clipboard
3. Open Git-Extension
4. Open the solution
5. Open the menu `Repository` and select `Manage submodules...`
6. Click `Add submodule`
7. Paste the URL from the clipboard into `Path to submodule`
8. **Prefix the local path with either `app/` or `shared/`**
9. Select the appropiate branch, e.g. `main`
10. Click `Add`
11. edit the `package.json` to have a qualified package name
12. In a solution folder terminal execute:
    1. `yarn clean-node-modules`
    2. `lerna bootstrap`
