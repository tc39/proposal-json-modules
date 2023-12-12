# JSON modules

Champions: Sven Sauleau ([@xtuc](https://github.com/xtuc)), Daniel Ehrenberg ([@littledan](https://github.com/littledan)), Myles Borins ([@MylesBorins](https://github.com/MylesBorins)), and Dan Clark ([@dandclark](https://github.com/dandclark))

Status: Stage 3.

Please leave any feedback you have in the [issues](http://github.com/tc39/proposal-json-modules/issues)!

## Synopsis

The JSON modules proposal builds on the [import attributes proposal](https://github.com/tc39/proposal-import-attributes/) to add the ability to import a JSON module in a common way across JavaScript environments.

Developers will then be able to import a JSON module as follows:
```js
import json from "./foo.json" with { type: "json" };
import("foo.json", { with: { type: "json" } });
```

Note: this proposal was originally part of the [import attributes proposal](https://github.com/tc39/proposal-import-attributes/), but it was [resolved](https://github.com/tc39/notes/blob/master/meetings/2020-07/july-21.md#import-conditions-for-stage-3) during the July 2020 meeting to split it out into a separate proposal.

## Motivation

Standards-track JSON ES modules were [proposed](https://github.com/w3c/webcomponents/issues/770) to allow JavaScript modules to easily import JSON data files, similarly to how they are supported in many nonstandard JavaScript module systems. This idea quickly got broad support from web developers and browsers, and was merged into HTML, with an implementation for V8/Chromium created by Microsoft.

However, in [an issue](https://github.com/w3c/webcomponents/issues/839), Ryosuke Niwa (Apple) and Anne van Kesteren (Mozilla) proposed that security would be improved if some syntactic marker were required when importing JSON modules and similar module types which cannot execute code, to prevent a scenario where the responding server unexpectedly provides a different MIME type, causing code to be unexpectedly executed. The solution was to somehow indicate that a module was JSON, or in general, not to be executed, somewhere in addition to the MIME type.  [Import attributes](https://github.com/tc39/proposal-import-attributes/) provide a mechanism for doing so, allowing us to reintroduce JSON modules.

Rather than granting hosts free reign to implement JSON modules independently, specifying them in TC39 guarantees that they will behave consistently across all ECMA262-compliant hosts.

## Proposed semantics and interoperability

If a module import has an attribute with key `type` and value `json`, the host is required to either fail the import, or treat it as a JSON module. Specifically this means that the content of the module is parsed as JSON and the resulting JSON object is the default export of the module (which has no named exports).

Each JavaScript host is expected to provide a secondary way of checking whether a module is a JSON module. For example, on the Web, the MIME type would be checked to be a JSON MIME type. In "local" desktop/server/embedded environments, the file extension may be checked (possibly after symlinks are followed). The `type: "json"` is indicated at the import site, rather than *only* through that other mechanism in order to prevent the privilege escalation issue noted in the opening section.

All of the import statements in the module graph that address the same JSON module will evaluate to the same mutable object as discussed in [#54](https://github.com/tc39/proposal-import-attributes/issues/54).

Nevertheless, the interpretation of module loads with no attributes remains host/implementation-defined, so it is valid to implement JSON modules without *requiring* `with { type: "json" }`. It's just that `with { type: "json" }` must be supported everywhere. For example, it will be up to Node.js, not TC39, to decide whether import attributes are required or optional for JSON modules.

Further attributes and module types beyond `json` modules could be added in future TC39 proposals as well as by hosts. HTML and CSS modules are also under consideration, and these may use similar explicit `type` syntax when imported.

## FAQ

### How would this proposal work with caching?

The determination of whether the `type` attribute is part of the module cache key is left up to hosts (as it is for all import attributes).

### Why don't JSON modules support named exports?

"Named exports" for each top-level property of the parsed JSON value were [considered](https://github.com/whatwg/html/issues/4315#issuecomment-456838848) in earlier HTML efforts towards JSON modules. On one hand, named exports are implemented by certain JavaScript tooling, and some developers find them to be more ergonomic/friendly to certain forms of tree shaking. However, they are not selected in this proposal because:
- They are not fully general: not all JSON documents are objects.
- It makes sense to think of a JSON document as conceptually ["a single thing"](https://github.com/whatwg/html/issues/4315#issuecomment-456838848) rather than several things that happen to be side-by-side in a file.

## Specification

* [Specification Outline](https://tc39.es/proposal-json-modules/)
* [Pull Request for HTML spec integration](https://github.com/whatwg/html/pull/5658)
