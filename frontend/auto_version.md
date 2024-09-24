# Automatic versioning in angular projects

> To maintain the track for the release and confirm the deployment visually in UI

## Description

Angular provides versioning for the projects where we can maintain the version and also change them during every build automatically.
The advantage of this are

-   Developer can confirm wether the latest code is deployed or not
-   can back track the changes and revert it.

## Steps

1. Create a environment file with name `environment.ts` and store the version number in it which will take from the package.json

```
export const environment = {
    app_version: require('../../package.json').version,
};
```

2. In `package.json` check if `@types/node` is installed and if not install it.

```
 "devDependencies": {
    ...
    "@types/node": "^22.5.5",
    ...
  }

```

3. Add prebuild in script section of the `package.json`

```
"scripts": {
    ...
    "prebuild": "npm --no-git-tag-version version patch",
    ...
}
```

> This is the main code which will auto increment the version while building the project.

## Working of Auto versioning

-   The versioning is maintained in `package.json` in a key called version

```
"version": "1.0.0"
```

-   As mentioned prebuild script is the main code which will auto increment the version while building the project.
-   To build the project we will be using `npm run build` but before this command we are telling node to run the prebuild script which will auto increment the version.
-   It will increment the npm version of the project based on the tag we give at the last
-   Version will have 3 numbering section which are separated by “.” dot
-   For example `1.1.1` which means `major . minor . patch`
-   Versioning will be incremented based on the tag we give at the last and it can be done using 3 tags
    -   `patch` - this will increment the last number
    -   `minor` - this will increment the middle number
    -   `major` - this will increment the first number
-   The tag `--no-git-tag-version` is given so that when the version is updated don't commit the changes and we will manually commit it once the deployment is done. This is because we deploy in ubuntu and if we need to commit we need to setup the configured email and username which can be complicated

> Rember to commit the changes once the deployment is done with the updated version

:artist: [Vivek V Pai](https://www.linkedin.com/in/vivek-v-pai-6b674b1b8/)
