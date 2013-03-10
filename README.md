# complexityReport.js

[![Build status][ci-image]][ci-status]

A tool for reporting code complexity metrics in JavaScript projects.
Currently the tool reports on:

* lines of code;
* cyclomatic complexity;
* Halstead metrics;
* maintainability index.

[Here is an example report][eg].

The tool can be configured to fail
when complexity metrics pass a specified threshold,
to aid its usefulness in automated environments / CI.
There are also options
for controlling how metrics are calculated
and the format of the report output.

The metrics are calculated by walking syntax trees
generated by the [Esprima] parser.

For people who are only interested in analysing small amounts of code
and don't want to download the tool,
there is also a web front-end available:

* [JSComplexity.org][jscomplexity]

## Installation

### Local to the current project

```
npm install complexity-report
```

### Globally for all projects

```
sudo npm install -g complexity-report
```

## Usage

### From the command line

```
cr [options] <file...>
```

The tool will recursively read files
from any directories that it encounters
automatically.

#### Options

* `-o <file>`: Specify an output file for the report,
  defaults to `stdout`.
* `-f <format>`: Specify an output format for the report,
  defaults to `plain`.
* `-a`: Include hidden files in the report.
* `-p` <regex>`: Specify the files to be processed
  using a regular expression to match against file names,
  defaults to `\.js$`.
* `-x <number>`: Specify the maximum number of files to open concurrently,
  defaults to `0`.
* `-m <maintainability>`: Specify the per-module maintainability index threshold
  (beyond which, the process will fail when exiting).
* `-c <complexity>`: Specify the per-function cyclomatic complexity threshold
  (beyond which, the process will fail when exiting).
* `-d <difficulty>`: Specify the per-function Halstead difficulty threshold
  (beyond which, the process will fail when exiting).
* `-v <volume>`: Specify the per-function Halstead volume threshold
  (beyond which, the process will fail when exiting).
* `-e <effort>`: Specify the per-function Halstead effort threshold
  (beyond which, the process will fail when exiting).
* `-s`: Silences the console output.
* `-l`: Disregards operator `||` as a source of cyclomatic complexity.
* `-w`: Disregards `switch` statements as a source of cyclomatic complexity.
* `-i`: Treats `for`...`in` loops as a source of cyclomatic complexity.
* `-t`: Treats `catch` clauses as a source of cyclomatic complexity.
* `-n`: Uses the [Microsoft-variant maintainability index][msvariant].

#### Output formats

Currently there are five output formats supported:
`plain`,
`markdown`,
`minimal`,
`json`
and `xml`.
These are loaded with `require`
from the `src/formats` subdirectory.
If the format file is not found
in that directory,
a second attempt will be made to load the module
without the subdirectory prefix.
Adding new formats is really easy;
each format module must export a function `format`,
which takes a report object as its only argument
and returns its string representation of the report.
See `src/formats/plain.js` for an example format.

### From code

#### Loading the library

```
var cr = require('complexity-report');
```

#### Calling the library

```
var report = cr.run(source, options);
```

The argument `source` must be a string
containing the source code that is to be analysed.
The argument `options` is an optional object
which may contain properties that modify
cyclomatic complexity calculation.
The following options are available:

* `logicalor`: Boolean indicating whether operator `||`
  should be considered a source of cyclomatic complexity,
  defaults to `true`.
* `switchcase`: Boolean indicating whether `switch` statements
  should be considered a source of cyclomatic complexity,
  defaults to `true`.
* `forin`: Boolean indicating whether `for`...`in` loops
  should be considered a source of cyclomatic complexity,
  defaults to `false`.
* `trycatch`: Boolean indicating whether `catch` clauses
  should be considered a source of cyclomatic complexity,
  defaults to `false`.
* `newmi`: Boolean indicating whether the maintainability
  index should be rebased on a scale from 0 to 100.

The returned report is an object
that contains properties detailing the complexity
of each function from the source code.
There is also
a maintainability index
as well as aggregate complexity metrics
for the source in its entirety.

## Related projects

Visualizations:

* [Gleb Bahmutov][gleb]'s [js-complexity-viz];
* [Jarrod Overson][jarrod]'s [Plato].

Build tools:

* [Viget Labs][viget]' [grunt-complexity];
* [Cliffano Subagio][cliffano]'s [Bob];
* [Cardio].

## Development

### Roadmap

The short-term plan is
to write more output formats
and open up lots more options
for external configuration of the analysis.

I also need to spend some time
throwing more complex test cases at it,
to flush out all of the edge cases
that I'm probably not yet handling.
To this end,
it would be great to hear from people
that have run the tool
against their own codebases.
The bigger and uglier, the better!
If you spot any issues,
please raise them in the [tracker].

In the longer term,
I have some vague ideas concerning
how to track trends in a codebase over time.
Visualisations is another area that could be pretty sweet to look into.

If you think there's anything else I should look at,
please raise an issue or, even better,
feel free to implement it and submit a pull request! `:)`

### Dependencies

The build environment relies on
[Node.js][node],
[NPM],
[Jake],
[JSHint],
[Mocha] and
[Chai].
Assuming that you already have Node.js and NPM set up,
you just need to run `npm install`
to install all of the dependencies
as listed in `package.json`.

### Tests

The tests are in `test/complexityReport.js`. You can run them with the
command `npm test` or `jake test`.

[ci-image]: https://secure.travis-ci.org/philbooth/complexityReport.js.png?branch=master
[ci-status]: http://travis-ci.org/#!/philbooth/complexityReport.js
[eg]: https://github.com/philbooth/complexityReport.js/blob/master/SELF.md
[esprima]: http://esprima.org/
[jscomplexity]: http://jscomplexity.org/
[msvariant]: http://blogs.msdn.com/b/codeanalysis/archive/2007/11/20/maintainability-index-range-and-meaning.aspx
[gleb]: https://github.com/bahmutov
[js-complexity-viz]: https://github.com/bahmutov/js-complexity-viz
[jarrod]: http://jarrodoverson.com/blog/about
[plato]: https://github.com/jsoverson/plato
[viget]: http://viget.com/
[grunt-complexity]: https://github.com/vigetlabs/grunt-complexity
[cliffano]: http://blog.cliffano.com/
[bob]: https://github.com/cliffano/bob
[cardio]: https://github.com/auchenberg/cardio
[tracker]: https://github.com/philbooth/complexityReport.js/issues
[node]: http://nodejs.org/
[npm]: https://npmjs.org/
[jake]: https://github.com/mde/jake
[jshint]: https://github.com/jshint/node-jshint
[mocha]: http://visionmedia.github.com/mocha
[chai]: http://chaijs.com/

