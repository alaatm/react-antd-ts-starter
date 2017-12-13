
* `create-react-app myapp --scripts-version=react-scripts-ts`
* `yarn add antd --save`
* `yarn add react-app-rewired react-app-rewire-less ts-import-plugin --dev`
* Add `config-overrides.js` file.
```javascript
const tsImportPluginFactory = require('ts-import-plugin')
const { getLoader } = require('react-app-rewired');
const rewireLess = require('react-app-rewire-less');

module.exports = function override(config, env) {
  const tsLoader = getLoader(
    config.module.rules,
    rule =>
      rule.loader &&
      typeof rule.loader === 'string' &&
      rule.loader.includes('ts-loader')
  );

  tsLoader.options = {
    getCustomTransformers: () => ({
      before: [ tsImportPluginFactory({
        libraryName: 'antd',
        libraryDirectory: 'es',
        style: true,
      }) ]
    })
  };

  config = rewireLess.withLoaderOptions({
    modifyVars: { 
      '@primary-color': '#1DA57A',  // Override primary color
      '@font-family': 'Tahoma'      // Override font name
      // ... more overrides
    },
  })(config, env);

  return config;
}
```
* Update `package.json`
```diff
"scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start --scripts-version react-scripts-ts",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build --scripts-version react-scripts-ts",
-   "test": "react-scripts test --env=jsdom",
+   "test": "react-app-rewired test --env=jsdom --scripts-version react-scripts-ts",
}
```