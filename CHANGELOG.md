# Change log

All notable changes to the `launchdarkly-js-sdk-common` package will be documented in this file. Changes that affect the dependent SDKs such as `launchdarkly-js-client-sdk` should also be logged in those projects, in the next release that uses the updated version of this package. This project adheres to [Semantic Versioning](http://semver.org).

## [3.1.1] - 2020-01-15
### Fixed:
- The SDK now specifies a uniquely identifiable request header when sending events to LaunchDarkly to ensure that events are only processed once, even if the SDK sends them two times due to a failed initial attempt.

## [3.1.0] - 2019-12-13
### Added:
- Configuration options `wrapperName` and `wrapperVersion`.
- Platform option `httpFallbackPing` (to be used for the browser image mechanism - see below).

### Fixed:
- When calling `identify`, the current user (as reported by `getUser()`) was being updated before the SDK had received the new flag values for that user, causing the client to be temporarily in an inconsistent state where flag evaluations would be associated with the wrong user in analytics events. Now, the current-user state will stay in sync with the flags and change only when they have finished changing. (Thanks, [edvinerikson](https://github.com/launchdarkly/js-sdk-common/pull/3)!)

### Removed:
- Logic for sending a one-way HTTP request in a browser by creating an image has been moved to the browser-specific code (`js-client-sdk`).


## [3.0.0] - 2019-12-13
### Added:
- Configuration property `eventCapacity`: the maximum number of analytics events (not counting evaluation counters) that can be held at once, to prevent the SDK from consuming unexpected amounts of memory in case an application generates events unusually rapidly. In JavaScript code this would not normally be an issue, since the SDK flushes events every two seconds by default, but you may wish to increase this value if you will intentionally be generating a high volume of custom or identify events. The default value is 100.

### Changed:
- (Breaking change) The `extraDefaults` parameter to the internal common `initialize` method is now `extraOptionDefs` and has a different format, allowing for more flexible validation.
- The SDK now logs a warning if any configuration property has an inappropriate type, such as `baseUri:3` or `sendEvents:"no"`. For boolean properties, the SDK will still interpret the value in terms of truthiness, which was the previous behavior. For all other types, since there's no such commonly accepted way to coerce the type, it will fall back to the default setting for that property; previously, the behavior was undefined but most such mistakes would have caused the SDK to throw an exception at some later point.
- Removed or updated some development dependencies that were causing vulnerability warnings.

### Deprecated:
- The `samplingInterval` configuration property was deprecated in the code in the previous minor version release, and in the changelog, but the deprecation notice was accidentally omitted from the documentation comments. It is hereby deprecated again.


## [2.14.1] - 2019-11-04
### Fixed:
- Removed uses of `Object.assign` that caused errors in Internet Explorer unless a polyfill for that function was present.



Prior to the 2.15.0 release, this code was a monorepo subpackage in the [`js-client-sdk`](https://github.com/launchdarkly/js-client-sdk) repo. See the [changelog](https://github.com/launchdarkly/js-client-sdk/blob/2.14.0/CHANGELOG.md) in that repo for changes prior to that version. It is now maintained in this repo and has its own versioning and changelog.
