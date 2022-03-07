Unreleased Changes
------------------

* Issue - Use `JSON.parse` instead of `JSON.load`.

1.6.0 (2022-02-14)
------------------

* Feature - Add support for string comparisons.

1.5.0 (2022-01-10)
------------------

* Support implicitly convertible objects/duck-type values responding to `to_hash` and `to_ary`.

  [See related GitHub pull request #51](https://github.com/jmespath/jmespath.rb/pull/51).

1.4.0 (2018-04-04)
------------------

* Update the bundled compliance tests. Fix the 6 failing test cases that result
  from updating the test suite. Test failures included parsing errors and
  returning nil when comparing non-nil values.

* Fix typo of a license name in gemspec.

  [See related GitHub pull request #41](https://github.com/jmespath/jmespath.rb/pull/41)

* Test against Ruby 2.4 in Travis.

  [See related GitHub pull requests #42](https://github.com/jmespath/jmespath.rb/pull/42)

* Add support for floating point comparisons.

  [See related GitHub pull requests #43](https://github.com/jmespath/jmespath.rb/pull/43)

1.3.1 (2016-07-18)
------------------

* Bug fix for users that have a 2.0.1+ version of the `json_pure` gem loaded
  in their environment prior to requiring `jmespath`.

1.3.0 (2016-07-07)
------------------

* Restored support for legacy unquoted string literals.

  [See related GitHub pull request #32](https://github.com/jmespath/jmespath.rb/pull/32).

* Improved error handling for invalid JSON values.

  [See related GitHub pull request #29](https://github.com/jmespath/jmespath.rb/pull/29).

* Optimised false checks.

  [See related GitHub pull request #24](https://github.com/jmespath/jmespath.rb/pull/24).

* Removed depdendency on `pure_json` gem. Necessary code changes have been
  made to ensure things work properly with Ruby 1.9.3 and JSON 1.5.5.

* Bug-fix for Ruby 2.3. JMESPath requires sort and sort_by functions to be stable.
  There was a persistent test failure in Ruby 2.3 due to an unstable sort.

1.2.4 (2016-04-06)
------------------

* Will no longer require json_pure if the json gem has already been loaded.
  This will result in a warning and a degraded experience if json < 1.8.1
  has already been loaded.

  Mixing json/pure with json/ext results in json errors, for example:

  ```ruby
  some_hash = { 'jsonrpc' => 'abc', 'jsonversion' => 1 }
  some_hash.to_json
  #=> raises a JSON::Pure::Generator::State TypeError
  ```

  [See related GitHub issue #20](https://github.com/jmespath/jmespath.rb/issues/20).

1.2.2 (2016-03-31)
------------------

* Removed hard dependency on `json >= 1.8.1`. Replaced with `json_pure >= 1.8.1`.
  The runtime will still attempt to load the faster gem, if availble and will fall
  back on `json_pure` for compatability. Ruby 2.0+ ships with 1.8.1 by default,
  so only Ruby 1.9.3 will default to the slower version.

  [See related GitHub issue #18](https://github.com/jmespath/jmespath.rb/pull/18).

1.2.1 (2016-03-31)
------------------

* Minor change in require order.

1.2 (2016-03-30)
------------------

* Errors caused by invalid function arguments normally raise
  an arity or argument error. You can now prevent `JMESPath.search`
  from raising these errors by passing `disable_visit_error: true`.

  ```
  JMESPath.search(expression, data, disable_visit_errors: true)
  ```

  This will cause these functions to return a null/nil value instead.

  [See related GitHub issue #10](https://github.com/jmespath/jmespath.rb/pull/10).

* Fix for Ruby 1.9.3. Older versions of Ruby ship with a version of the json
  gem that can not perform the following:

  ```ruby
  JSON.load('1')
  ```

  This results in the JMESPath library in assuming is parsing an unknown or
  invalid token. This works fine newer versions of Ruby. To resolve this issue
  the library is forcing a newer version of the `json` gem.

  [Fixes GitHub issue #11](https://github.com/jmespath/jmespath.rb/issues/11).

* Fix for boolean truthy checks.
  [See related GitHub issue #15](https://github.com/jmespath/jmespath.rb/pull/15).

* Updated code to pass the latest shared compliance tests.

* Added support for the `map` function.

* Added support for [JEP-9](https://github.com/jmespath/jmespath.site/blob/master/docs/proposals/improved-filters.rst),
  including unary filter expressions, and `&&` filter expressions.

1.1.3 (2015-09-16)
------------------

* Removed json gem dependency.

1.1.2 (2015-09-16)
------------------

* Resolved an issue preventing eager autoloading of comparator classes.

1.1.1 (2015-09-16)
------------------

* Fix for Ruby version 1.9.3 which does not support `#[]`
  on `Enumerable` from Ruby stdlib.

1.1.0 (2015-09-16)
------------------

* Updated the compliance tests. Pulled in the latest version from
  https://github.com/jmespath/jmespath.test/commit/47fa72339d0e5a4d0e9a12264048fc580ed0bfd8.

* Adds a new JIT-friendly interpreter and AST optimizations that evaluating
  expressions faster.

  See [related GitHub pull request](https://github.com/jmespath/jmespath.rb/pull/4).

* Removed dependency on `multi_json`.

* Now running compliance tests as part of release process.

1.0.2 (2014-12-04)
------------------

* Added a copy of the Apache 2.0 license to the project and now
  now bundling the license as part of the release.

1.0.1 (2014-10-28)
------------------

* Bug-fix, when accessing Struct objects with an invalid member
  `nil` is now returned, instead of raising an error.

1.0.0 (2014-10-28)
------------------

* The expression cache now has a maximum size.

* Documented the `rake benchmark` and `rake benchmark:cached` tasks.

* You can now disable expression caching when constructing a Runtime by
  passing `:cache_expressions => false`. Caching is still enabled by
  default.

  ```ruby
  # disable caching
  runtime = JMESPath::Runtime.new(cache_expressions: false)
  runtime.search(expression, data)
  ```

* Adding a missing require statement for Pathname to the JMESPath module.

0.9.0 (2014-10-27)
------------------

* Addded support for searching over hashes with symbolized keys and Struct
  objects indifferently

  ```ruby
  # symbolized hash keys
  data = {
     foo: {
      bar: {
        yuck: 'value'
      }
    }
  }
  JMESPath.search('foo.bar.yuck', data)
  #=> 'value'

  # Struct objects
  data = Struct.new(:foo).new(
    Struct.new(:bar).new(
      Struct.new(:yuck).new('value')
    )
  )
  JMESPath.search('foo.bar.yuck', data)
  #=> 'value'
  ```

* Added a simple thread-safe expression parser cache; This significantly speeds
  up searching multiple times with the same expression. This cache is enabled
  by default when calling `JMESPath.search`

* Added simple benchmark suite. You can execute benchmarks with with `rake benchmark`
  or `CACHE=1 rake benchmark`; Caching is disabled by default in the benchmarks.

0.2.0 (2014-10-24)
------------------

* Passing all of the JMESPath compliance tests.
