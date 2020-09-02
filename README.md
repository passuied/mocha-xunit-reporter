# mocha-xunit-reporter

A Mocha xunit reporter. Produces XUnit-style XML test results.
Adapted from and Inspired by [mocha-junit-reporter](https://github.com/michaelleeallen/mocha-junit-reporter).

## Installation

```
$ npm install mocha-xunit-reporter --save-dev
```
or as a global module
```
$ npm install -g mocha-xunit-reporter
```

## Usage

Run mocha with `mocha-xunit-reporter`:
```
$ mocha test --reporter mocha-xunit-reporter
```
or, in `.mocharc.json` (or the exported object from `.mocharc.js`):
```json
{
    "reporter": "mocha-xunit-reporter",
    ...
}
```

## Output

### Format

The generated test results file conforms to [XUnit's XML format](https://xunit.net/docs/format-xml-v2).

### `failure` element
In case of an error or failure during a test run, a `failure` element will be included as a child element with its corresponding `test` element. This element will contain information about the failed test.

```xml
<failure exception-type="Error">
  <message><![CDATA[This test threw an error]]></message>
  <stack-trace><![CDATA[Error: testing123
at Context.<anonymous> (example.spec.ts-1:1:1)]]></stack-trace>
</failure>
```

[More information](https://xunit.net/docs/format-xml-v2#failure) about the `failure` element.


## Configuration

Configuration options (all optional):

| Parameter | Type | Effect |
| --------- | ---- | ------ |
| `mochaFile` | string | configures the file to write reports to |
| `includePending` | boolean | if set to a truthy value pending tests will be included in the report |
| `toConsole` | boolean | if set to a truthy value the produced XML will be logged to the console |
| `assemblyName` | string | the name for the assembly element. (defaults to 'Mocha Tests') |
| `addTags` | boolean | if set to a truthy value will parse the test title for tags |

### Specifying Reporter Options

In general, configuration options may be specified any of the following ways. There may be additional ways to specify individual options (see below).

- Command-line reporter options; e.g.
    ```js
    $ mocha test --reporter mocha-xunit-reporter --reporter-options mochaFile=./path_to_your/file.xml
    ```

- Mocha constructor `reporterOptions`; e.g.
    ```js
    var mocha = new Mocha({
        reporter: 'mocha-xunit-reporter',
        reporterOptions: {
            mochaFile: './path_to_your/file.xml'
        }
    });
    ```

- `.mocharc.js`/`.mocharc.json`:
    ```json
    {
        "reporter": "mocha-xunit-reporter",
        "reporterOptions": {
            "mochaFile": "./path_to_your/file.xml"
        }
    }
    ```

Details about some configuration options are included below.

---

### `mochaFile` or `MOCHA_FILE`

The generated test results file conforms to [XUnit's XML format](https://xunit.net/docs/format-xml-v2).

Default location: `./test-results.xml`

This option may additionally be specified by the environment variable `MOCHA_FILE`:

```js
$ MOCHA_FILE=./path_to_your/file.xml mocha test --reporter mocha-xunit-reporter
```

#### Magic filename

If the string `[hash]` is present in the received file path, it will be replaced with an MD5 hash of the output XML.

For example, for this received file path:
```
test-results.[hash].xml
```
...the output file might be `test-results.320ae2121e02b35c30dc16b8b7a2215e.xml`

### `addTags`

If set to true, will parse the test title for tags in format `@tagName=tagValue` and will add them as attribute of the test XML element. It will also clean the outputted tags from the test name XML attribute.

#### Example behavior:

Consider a test with this title:
```js
'test should behave like so @aid=EPM-DP-C1234 @sid=EPM-1234 @type=Integration'
```

The outputted test element will look as follows:
```xml
<test name="test should behave like so" aid="EPM-DP-C1234" sid="EPM-1234" type="Integration" />
```
