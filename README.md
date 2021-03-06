# react-native-typescript-transformer

A transformer to use for loading TypeScript files with react-native >= 0.45

It currently uses Babel as a secondary compilation step for simplicity's sake, and to
enable synthetic default imports. A planned feature is to allow bypassing babel
for people who don't use synthetic default imports.

## Usage

### Step 1: Install

    yarn add --dev react-native-typescript-transformer typescript

### Step 2: Configure TypeScript

Make sure your tsconfig.json has the following:

```json
{
  "compilerOptions": {
    "target": "es2015",
    "module": "es2015",
    "jsx": "react-native",
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true
  }
}
```

#### Notes

`"module"` can be `"commonjs"` if you don't care about allowing synthetic default imports (in which case that field can also be `false`)

`"target"` can probably be anything supported by your babel setup, I suppose.

`"jsx"` can also be `"preserve"`, they are functionally identical if you don't emit files.

### Step 3: Configure the react native packager

Add this to your rn-cli.config.js (make one if you don't have one already):

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve('react-native-typescript-transformer')
  },
  getSourceExts() {
    return ['ts', 'tsx'];
  }
}
```

If you are using **create-react-native-app** You need to tell Expo to look for the
cli config file. Add the `packagerOpts.config` property to your app.json, e.g.

```json
{
  "expo": {
    "sdkVersion": "18.0.0",
    "packagerOpts": {
      "config": "rn-cli.config.js"
    }
  }
}
```

If you need to run the packager directly from the command line, pass these

    --transformer node_modules/react-native-typescript-transformer --sourceExts ts,tsx

### Step 4: Write TypeScript code :D

Note that the platform-specific index files (index.ios.js, index.android.js, etc)
still need to be .js files, but everything else can be TypeScript.


## License

MIT

[![Empowered by Futurice's open source sponsorship program](https://img.shields.io/badge/sponsor-futurice-ff69b4.svg)](http://futurice.com/blog/sponsoring-free-time-open-source-activities?utm_source=github&utm_medium=spice&utm_campaign=react-native-typescript-transformer)
