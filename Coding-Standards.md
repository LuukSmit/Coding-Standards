# eslint-config-travpro

ESLint configuration for all of TravPRO projects

<!-- DO NOT EDIT: AUTO-GENERATED CONTENT -->

# Table of Contents
  
[1. Rules](#1-rules)  
&nbsp;&nbsp;&nbsp;&nbsp; [import/extensions](#importextensions)  
&nbsp;&nbsp;&nbsp;&nbsp; [import/no-cycle](#importno-cycle)  
&nbsp;&nbsp;&nbsp;&nbsp; [import/no-extraneous-dependencies](#importno-extraneous-dependencies)  
&nbsp;&nbsp;&nbsp;&nbsp; [max-lines](#max-lines)  
&nbsp;&nbsp;&nbsp;&nbsp; [jsx-a11y/anchor-is-valid](#jsx-a11yanchor-is-valid)  
&nbsp;&nbsp;&nbsp;&nbsp; [max-len](#max-len)  
&nbsp;&nbsp;&nbsp;&nbsp; [no-console](#no-console)  
&nbsp;&nbsp;&nbsp;&nbsp; [react/jsx-curly-brace-presence](#reactjsx-curly-brace-presence)  
&nbsp;&nbsp;&nbsp;&nbsp; [react/jsx-filename-extension](#reactjsx-filename-extension)  
&nbsp;&nbsp;&nbsp;&nbsp; [react/jsx-fragments](#reactjsx-fragments)  
&nbsp;&nbsp;&nbsp;&nbsp; [react/jsx-indent](#reactjsx-indent)  
&nbsp;&nbsp;&nbsp;&nbsp; [react/jsx-props-no-spreading](#reactjsx-props-no-spreading)  

<!-- DO NOT EDIT: AUTO-GENERATED CONTENT -->

# 1. Rules

## import/extensions

**[Ensure consistent use of file extension within the import path](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)**

Due to how common module imports are we've decided to exclude `.js` files (components, so this includes `.jsx` files) as well as `.json` files (which are imported as a JavaScript object) from having their extension included. The paths, even without their extension, are automatically resolved.

## import/no-cycle

**[Ensures that there is no resolvable path back to this module via its dependencies](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-cycle.md)**

This rule was changed in order to use a maxDepth of 1. This allows this rule to still be in effect without unnecessarily flagging C4 components. Due to the way Webpack is setup the C4 file does not actually cause an import cycle. However since ESLint cannot detect this and this rule is still valuable in other cases we've opted not to disable it altogether.

**Note:** This rule may still flag imports in the C4 file sometimes, usually manually added files. This is not an error and it's worth considering if the files actually need to be imported through C4.

## import/no-extraneous-dependencies

**[Forbid the import of external modules that are not declared in the package.json's dependencies, devDependencies, optionalDependencies, peerDependencies, or bundledDependencies.](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-extraneous-dependencies.md)**

This rule is changed in order to override the default behaviour of eslint-plugin-import. This allows the importing of devDependencies in pre-commit hooks and build / file generation scripts.

## max-lines

**[enforces a maximum number of lines per file, in order to aid in maintainability and reduce complexity](https://eslint.org/docs/rules/max-lines)**

Ensure files keep short and targeted. Long files are often confusing and more difficult to update.

## jsx-a11y/anchor-is-valid

**[Enforces the use of valid anchor tags that do not attempt to circumvent their intended use](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/anchor-is-valid.md)**

This rule is merely extended to include non-native components that need to be checked by this rule. For example React Router's `<Link />` component.

The option passed to this rule simply specifies that all sub-rules must be enforced.

## max-len

**[Enforce a maximum line length](https://eslint.org/docs/rules/max-len)**

This rule is exchanged to allow lines of 120 characters, up from 80, as we feel this is still short enough to be clear but allows for more freedom when writing code.

Comments are not subject to this rule as they are not part of the code.

This rule is passed an ignore pattern ([details](https://regexr.com/58ia1)) that allow the strings in language files to be exempt from this rule. These strings are often very long and the purpose of their files are simply to contain the data. Therefore limiting their length comes with no real advantage. The ignore pattern was chosen over the `ignoreStrings` option as we still need strings in components to be limited.

## no-console

**[disallow the use of console](https://eslint.org/docs/rules/no-console)**

By default this rule disallows the use of any `console` method. These messages are used for debugging and therefore there's no real need to include them in a build. Although Webpack is configured to strip `console` methods from the bundle, these methods unnecessarily clutter code and even the console in other parts of the application.

This rule was changed to allow for the use of the `console.warn` and `console.error` methods, since these are often used for general error logging. The `console.info` is also excluded from the rule, it can be used to log important information to the console in a case where `warn` and `error` don't apply.

All other console methods are considered warnings.

## react/jsx-curly-brace-presence

**[Enforce curly braces or disallow unnecessary curly braces in JSX props and/or children.](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-brace-presence.md)**

This rule was changed to enforce the use of curly braces around `children`. Generally speaking `children` should either be another component / node, a variable, or a (non-variable) string. Since this rule only applies to non-variable strings, since all other types _require_ curly braces, not enforcing curly braces leads to inconsistencies.

**Note:** Generally speaking you never have to use non-variable strings as `children`. If you need to pass a string to a component as `children` you should use an [`i18n`](https://www.npmjs.com/package/i18n-react) translation key.

## react/jsx-filename-extension

**[Restrict file extensions that may contain JSX](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)**

The default rule specifies `.jsx` as the only file extension that allows the use of JSX. Since this rule is largely a matter of preference we've decided to include `.js` as one of the allowed file extensions.

## react/jsx-fragments

**[Enforce shorthand or standard form for React fragments](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-fragments.md)**

Since this rule is just intended to enforce one or the other type of Fragment this rule also comes down to opinion. We've chosen to always enforce the standard form since the shorthand cannot take [keys](https://reactjs.org/docs/lists-and-keys.html).

## react/jsx-indent

**[Enforce consistent indentation style](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-indent.md)**

Overrides the default 4 spaces in favour of 2 spaces. Also changes the `checkAttributes` and `indentLogicalExpressions` default values in order to consistently indent JSX regardless of component style.

## react/jsx-props-no-spreading

**[Enforces that there is no spreading for any JSX attribute](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-props-no-spreading.md)**

We've decided to turn this rule off altogether since there are use cases for spreading props onto both native and custom tags. The most common use cases are when a component accepts a large number of props, or when it accepts all the properties of an element inside an iterable.

A less-common use case is when a large number of components accept the same props with slight variations.

**Note:** Care should be taken when spreading props onto an element in a loop. It's rare for the object to be fully spreadable onto the component, it's likely you'll want to destructure a number of properties and spread the remainder instead of the entire object.
