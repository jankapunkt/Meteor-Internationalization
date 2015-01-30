Reactive i18n and i10n isomorphic service for Meteor
========
File based and reactive internationalization isomorphic driver for Meteor with support of placeholders.

Install:
========
```shell
meteor add ostrio:i18n
```

### Files and Folders structure
```
 private/
 ├─ i18n/ //--> Driver's dir
 │  ├── en/ //--> Localization folder with name of country two-letter code
 │  ├─── file.json
 │  ├─── subFolder/ 
 │  |    └── anotherFile.json
 │  ├── en/ //--> Localization folder with name of country two-letter code
 │  ├─── file.json
 │  └─── subFolder/ 
 │       └── anotherFile.json
 └──i18n.json //--> Config file
```

This structure with sample data will be automatically added if file `private/i18n/i18n.json` is not exists

### Client usage
#### `get()` method
```javascript
/*
 * @function
 * @namespace i18n
 * @property {function} get          - Get values, and do pattern replaces from current localization
 * @param    {string}   param        - string in form of dot notation, like: folder1.folder2.file.key.key.key... etc.
 * @param    {mix}      replacements - Object, array, or string of replacements
 */
i18n.get(param, replacements)
```

#### `setLocale()` method
```javascript
/*
 * @function
 * @namespace i18n
 * @property {function} setLocale - Set locale (by ISO code)
 * @description Set new locale if it is configured in /private/i18n/i18n.json config file.
 *              Update session's and localStorage or cookie (via Meteor.storage) dependencies
 * @param {string} locale - Two letter locale code
 */
i18n.setLocale(locale)
```

#### Set locale
```javascript
i18n.setLocale('en');
```

#### Get value by key
```javascript
i18n.get('sample.hello');
```

#### Get value by key with single placeholder
```javascript
// Hi {{name}}!
i18n.get('sample.userHello', 'Michael');
```

#### Get value by key with multiply placeholders by Object
```javascript
// User's full name is: {{first}} {{middle}} {{last}}
i18n.get('sample.fullName', {first: 'Michael', middle: 'A.', last: 'Macht'});
```

#### Get value by key with multiply placeholders by Object, with wrong key
```javascript
// User's full name is: {{first}} {{middle}} {{last}}
i18n.get('sample.fullName', {first: 'Michael', middle: 'A.', wrong: 'Macht'});
```

#### Get value by key with multiply placeholders by Array
```javascript
// User's full name is: {{first}} {{middle}} {{last}}
i18n.get('sample.fullName', ['Michael', 'A.', 'Macht']);
```

#### Get configuration object
```javascript
i18n.config;
```

#### Get user's browser locale (preferred locale)
```javascript
i18n.userLocale;
```

#### Get current Client's locale
```javascript
i18n.currentLocale;
```

#### Get current default locale
```javascript
i18n.defaultLocale;
```

#### Get l10ns as object
```javascript
i18n.localizations;
```

### Server usage
*Note: * Server has no `setLocale()` method

#### `get()` method
```javascript
/*
 * @function
 * @namespace i18n
 * @property {function} get          - Get values, and do pattern replaces from current localization
 * @param    {string}   locale       - Two-letter localization code
 * @param    {string}   param        - string in form of dot notation, like: folder1.folder2.file.key.key.key... etc.
 * @param    {mix}      replacements - Object, array, or string of replacements
 */
i18n.get(loacale, param, replacements)
```

#### Get value by key
```javascript
i18n.get('en', 'sample.hello');
```

#### Get value by key with single placeholder
```javascript
// Hi {{name}}!
i18n.get('de', 'sample.userHello', 'Michael');
```

#### Get value by key with multiply placeholders by Object
```javascript
// User's full name is: {{first}} {{middle}} {{last}}
i18n.get('ru', 'sample.fullName', {first: 'Michael', middle: 'A.', last: 'Macht'});
```

#### Get value by key with multiply placeholders by Object, with wrong key
```javascript
// User's full name is: {{first}} {{middle}} {{last}}
i18n.get('de', 'sample.fullName', {first: 'Michael', middle: 'A.', wrong: 'Macht'});
```

### Session helpers
```javascript
Session.get('i18nCurrentLocale'); // Returns current Two-letter localization code
Session.get('i18nConfig'); // Returns array of configuration objects
```

Template helpers
================
`i18n` - accepts param and replacements:
```jade
p {{i18n 'sample.hello'}}
p {{{i18n 'sample.html'}}}
p {{i18n 'sample.fullName'}}
p {{i18n 'sample.fullName' first='Michael' middle='A.' last='Macht'}}
p {{i18n 'sample.fullName' first='Michael' middle='A.' third='Macht'}}
p {{i18n 'sample.fullName' 'Michael' 'A.' 'Macht'}}
```


#### Template language switcher example
```html
<template name="i18nSwitcher">
  <ul class="">
    {{#each Session 'i18nConfig'}}
      {{> i18nList}}
    {{/each}}
  </ul>
</template>

<template name="i18nList">
{{#if this.value.code}}
  {{#if isNotEqual this.value.code this.currentLocale}}
    <li>
      <a href="#" onclick="i18n.setLocale('{{this.value.code}}'); return false;">{{this.value.name}}</a>
    </li>
  {{/if}}
{{/if}}
</template>
```

Template helpers `isNotEqual`, `Session` and many more comes from: [ostrio:templatehelpers](https://atmospherejs.com/ostrio/templatehelpers) package