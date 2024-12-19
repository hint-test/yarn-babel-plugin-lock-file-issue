# Test an issue that when use yarn to install `@babel/plugin-transform-exponentiation-operator@7.22.5`, the `yarn.lock` file is not locked.

## Reproduce Steps
```
mkdir yarn-dep
cd yarn-dep
git init
yarn add @babel/plugin-transform-exponentiation-operator@7.22.5
git add ./package.json
git add ./yarn.lock
git commit -m 'init import'
rm -rf ./node_modules
yarn
```

You should see the `yarn.lock` file changed, which should not happen.

If you use `npm` as package manager, or install a different version if `@babel/plugin-transform-exponentiation-operator`,
the issue doesn't exist.

I guess the issue is related to `@babel/helper-builder-binary-assignment-operator-visitor`,
a dependency of `@babel/plugin-transform-exponentiation-operator`, which is removed later https://github.com/babel/babel/pull/16895.
But I'm not sure, because I tested on `v7.25.9`, it still has this dependency, but it has no issues.

## Folders
- `./yarn-dep/` `yarn add @babel/plugin-transform-exponentiation-operator` :negative_squared_cross_mark:
- `./yarn-dev-dep/` `yarn add @babel/plugin-transform-exponentiation-operator -D` :negative_squared_cross_mark:
- `./yarn-dev-dep-7.25.9/` `yarn add @babel/plugin-transform-exponentiation-operator@7.25.9 -D` :white_check_mark:
- `./yarn-dev-dep-latest/` `yarn add @babel/plugin-transform-exponentiation-operator@latest -D` :white_check_mark:
- `./npm-dep/` `npm install @babel/plugin-transform-exponentiation-operator` :white_check_mark:
- `./npm-dev-dep/` `npm install @babel/plugin-transform-exponentiation-operator -D` :white_check_mark:
- `./node14-yarn-dep/` Use Node.js 14.20 `yarn add @babel/plugin-transform-exponentiation-operator` :negative_squared_cross_mark:
