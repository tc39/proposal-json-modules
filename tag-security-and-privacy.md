# TAG Review: Security and Privacy questionnaire

## 2.1. What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?

Importing a module exposes the information to web servers that a particular resource is being fetched, which is necessary to load the module graph.  This proposal does not change or add to the information exposed in this manner.

## 2.2. Is this specification exposing the minimum amount of information necessary to power the feature?

Yes.

## 2.3. How does this specification deal with personal information or personally-identifiable information or information derived thereof?

HTML imports modules by performing fetches from the URL indicated in the module specifier. JavaScript code may construct a URL exposing personally identifying information which is implicitly communicated by importing a particular kind of JSON module, but this is already possible with JS modules.

No new identifying information is communicated by this proposal.

## 2.4. How does this specification deal with sensitive information

No extra sensitive information is exposed by this proposal.

## 2.5. Does this specification introduce new state for an origin that persists across browsing sessions?

No.

## 2.6. What information from the underlying platform, e.g. configuration data, is exposed by this specification to an origin?

If a host implements this proposal, JSON modules must be supported.

## 2.7. Does this specification allow an origin access to sensors on a user’s device

No.

## 2.8. What data does this specification expose to an origin? Please also document what data is identical to data exposed by other features, in the same or different contexts.

No data.

## 2.9. Does this specification enable new script execution/loading mechanisms?

This proposal will enable loading of (nonexecutable) JSON modules.

## 2.10. Does this specification allow an origin to access other devices?

No.

## 2.11. Does this specification allow an origin some measure of control over a user agent’s native UI?

No.

## 2.12. What temporary identifiers might this specification create or expose to the web?

No identifiers.

## 2.13. How does this specification distinguish between behavior in first-party and third-party contexts?

This specification allows importing cross-origin JSON document subresources as modules, analogous to how ES modules work. The imported subresource is not distinguished and generally treated as "first-party", but the explicit use of `type: "json"` avoids giving this subresource unnecessary capabilities (including both executing code and accessing parsers).

## 2.14. How does this specification work in the context of a user agent’s Private Browsing or "incognito" mode?

No difference.

## 2.15. Does this specification have a "Security Considerations" and "Privacy Considerations" section?

Part of the proposal motivation is the security aspect, as explained in the [README.md#motivation](https://github.com/tc39/proposal-import-conditions/blob/master/README.md#motivation).
We consider that there are no particular privacy considerations.

## 2.16. Does this specification allow downgrading default security characteristics?

No.

## 2.17. What should this questionnaire have asked?

The questions seem adequate.
