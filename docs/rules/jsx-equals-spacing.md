# Enforce or disallow spaces around equal signs in JSX attributes. (jsx-equals-spacing)

Some style guides require or disallow spaces around equal signs.

## Rule Details

This rule will enforce consistency of spacing around equal signs in JSX attributes, by requiring or disallowing one or more spaces before and after `=`.

### Options

There are two options for the rule:

* `"always"` enforces spaces around the equal sign
* `"never"` disallows spaces around the equal sign (default)

Depending on your coding conventions, you can choose either option by specifying it in your configuration:

```json
"jsx-equals-spacing": [2, "always"]
```

#### never

When `"never"` is set, the following patterns are considered warnings:

```js
<Hello name = {firstname} />;
<Hello name ={firstname} />;
<Hello name= {firstname} />;
```

The following patterns are not warnings:

```js
<Hello name={firstname} />;
<Hello name />;
<Hello {...props} />;
```

#### always

When `"always"` is used, the following patterns are considered warnings:

```js
<Hello name={firstname} />;
<Hello name ={firstname} />;
<Hello name= {firstname} />;
```

The following patterns are not warnings:

```js
<Hello name = {firstname} />;
<Hello name />;
<Hello {...props} />;
```

## When Not To Use It

You can turn this rule off if you are not concerned with the consistency of spacing around equal signs in JSX attributes.

