## WHAT DID YOU DO

<details>
<summary>Configuration</summary>

```js
[
  {
    rules: {
      "no-unused-vars": "error",
    },
  },
  {
    ignores: ["error.js"],
    rules: {
      "no-unused-vars": "warn",
    },
  },
];
```

</details>

`./error.js` and `./warn.js`:

```js
let unusedVar;
```

Then run `eslint .`

## EXPECTED

```shell
{cwd}/error.js
  1:5  error  'unusedVar' is defined but never used  no-unused-vars

{cwd}/warn.js
  1:5  warning  'unusedVar' is defined but never used  no-unused-vars
```

## HAPPENED

```shell
{cwd}/error.js
  1:5  warning  'unusedVar' is defined but never used  no-unused-vars

{cwd}/warn.js
  1:5  warning  'unusedVar' is defined but never used  no-unused-vars
```

## COMMENTS

I set 2 configuration objects: one with a rule all files and another to change that rule for all files EXCEPT `error.js`.
I omitted the `files` property in these objects; although the documentation recommends using `ignores` with `files`, [it states that this should work](https://eslint.org/docs/latest/use/configure/configuration-files-new#excluding-files-with-ignores) and omitting `files` should be the same as using `files: '**/*'`.

In reality, it works only if I explicitly set `files` in the second configuration object:

```js
[
  {
    rules: {
      "no-unused-vars": "error",
    },
  },
  {
    files: ["**/*"],
    ignores: ["error.js"],
    rules: {
      "no-unused-vars": "warn",
    },
  },
];
```
