# Introduction

This is an test webpart root workspace of a SharePoint MVC implementation using PNPjs, Classtransformer, MobX, Spfx-Controls and Fluent UI.

It's purpose is to test and showcase the technical state of the [reusable hybrid repo mvc spfx examples](https://github.com/mauriora/reusable-hybrid-repo-mvc-spfx-examples):

- clean invisible object state management with MobX
  - the webpart has no reference to Mobx it only deals with the data objects
  - all MobX use in the shared libraries
- state of the Views, fields and form
- access to a list on a different site
- starting point for webparts accessing lists on different sites

> focus on business logic development

## Table of contents

1. [Getting Started](#getting-started)
   1. [Requirements](#requirements)
   2. [Minimal path to awesomeness](#minimal-path-to-awesomeness)
2. [Build and install](#build-and-install)
3. [Content](#content)
   1. [Mobx state management](#mobx-state-management)
4. [Contribute](#contribute)
   1. [To do list](#to-do-list)
5. [Create new project](#create-new-project)

## Getting Started

You can use the "Minimal path to awesomeness" to have look around. Please branch before contributing.
You can follow the "Create a new project" to start with a copy of this a starting point for a new project (usually app)

### Requirements

You should have the following installed:

- A code editor
- Node 14
- yarn
- git
- GitExtension (the program, if your contributing or forking a new project of this)
- access to a SharePoint site with lists

### Minimal path to awesomeness

The lists are selected in the webpart configuration. They need to match the models in the app.

#### Models

[Edit properties of the models](./app/WebPart-Test/src/webparts/WebPart-Test/models) to match your lists. As example:

```typescript
export class TestList1 extends ListItem {
    public constructor() {
        super();
    }

    @Expose({ name: 'CustomText'})
    public customText: string;
    ...

    @Type( () => ListItemBase )
    @Expose({ name: 'SingleLookup' })
    public singleLookup: ListItemBase;

}
```

Extending from `ListItem` makes it on observable SharewPoint object.
It inherits properties like `author`, `contentTypeId`, ... The SharePoint

- internal fieldname `CustomText` is transformed to the property `customText`.
- `singleLookup` is using `ListItemBase` as a lookup to Title and Id.

The `Announcements` list can be located on a different site.

#### Build

```shell
git clone --recurse-submodules https://github.com/mauriora/WebPart-Test-Workspaces.git
cd WebPart-Test-Workspaces
yarn
code .
```

open a terminal in the solution (root) folder
The `node_modules` folder in the solution should contain the majority of dependencies, while the `node_modules` folder of each sub modules should only contain very few dependencies if any.
All modules in `shared` should be linked in the solutions `node_modules` folder.

```shell
yarn build-shared
yarn serve
```

[browse to the workbench on *YOUR-SITE*](https://YOUR-DOMAIN.sharepoint.com/sites/YOUR-SITE/_layouts/15/workbench.aspx)
add the webpart to the page

## Build and install

1. In a solution terminal execute `yarn workspace @mauriora/webpart-test serve`
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

## Content

The webpart displays two list from different sites to demonstrate cross site access:

```typescript
return <Stack>
        <ListAndForm
            key={'ListAndForm-local'}
            dataClass={TestList1}
            siteUrl={localSiteUrl}
            listId={localListId}
        />
        <ListAndForm
            key={'ListAndForm-isolated'}
            dataClass={Announcement}
            siteUrl={isolatedSiteUrl}
            listId={isolatedListId}
        />
    </Stack>;
```

### Mobx state management

Each *ListAndForm* contains a list with two forms next to each other. Each form showing the same object:

```typescript
    return <Stack>
            <ItemsList model={model} onSelect={onSelect} />
            <Stack horizontal>
                <ItemForm
                    key={listId + '-form1'}
                    model={model}
                    item={item}
                    ... />
                <ItemForm
                    key={listId + '-form2'}
                    model={model}
                    item={item}
                    ... />
            </Stack>
        </Stack>;
```

No explicit event handler exists between the forms.

a modification to the `item[property]` in either form is instantly reflected in the other form. Here the example of the boolean field:

```typescript
export const BooleanField: PropertyFieldFC = observer(({ info, item, property }) => {
    const value = item[property];
    if(undefined !== value && typeof value !== 'boolean') throw new Error(...);

    return <Checkbox
        label={info.Title}
        checked={value}
        disabled={info.ReadOnlyField}
        onChange={(e, checked) => item[property] = checked}
    />
});
```

`observer` is mobX rendering the checkbox when the observable `property` in the observable `item` changes. `item` is is an instance of your model. All properties in the modules are automatically observable. For the `observer` to work, the property needs to be accessed in side the observer.
The `info` object comes from the SharePoint model being a `IFieldInfo`, e.g. info.Title` is the diplayname.

## Contribute

Use the minmal path to awesomeness and please create a branch and fork for your contribution.
Then do a pull request to merge your branch into the main branch.

## Create new project

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

## Add a new sub module to the solution

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
