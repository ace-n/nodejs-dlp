<img src="https://avatars2.githubusercontent.com/u/2810941?v=3&s=96" alt="Google Cloud Platform logo" title="Google Cloud Platform" align="right" height="96" width="96"/>

# Google Cloud Data Loss Prevention (DLP) API: Node.js Client

[![release level](https://img.shields.io/badge/release%20level-beta-yellow.svg?style&#x3D;flat)](https://cloud.google.com/terms/launch-stages)
[![CircleCI](https://img.shields.io/circleci/project/github/GoogleCloudPlatform/google-cloud-node.svg?style=flat)](https://circleci.com/gh/GoogleCloudPlatform/google-cloud-node)
[![AppVeyor](https://ci.appveyor.com/api/projects/status/github/GoogleCloudPlatform/google-cloud-node?branch=master&svg=true)](https://ci.appveyor.com/project/GoogleCloudPlatform/google-cloud-node)
[![codecov](https://img.shields.io/codecov/c/github/GoogleCloudPlatform/google-cloud-node/master.svg?style=flat)](https://codecov.io/gh/GoogleCloudPlatform/google-cloud-node)

> Node.js idiomatic client for [Data Loss Prevention (DLP) API][product-docs].

The [Data Loss Prevention API](https://cloud.google.com/dlp/docs/) provides programmatic access to a powerful detection engine for personally identifiable information and other privacy-sensitive data in unstructured data streams.

* [Data Loss Prevention (DLP) API Node.js Client API Reference][client-docs]
* [Data Loss Prevention (DLP) API Documentation][product-docs]

Read more about the client libraries for Cloud APIs, including the older
Google APIs Client Libraries, in [Client Libraries Explained][explained].

[explained]: https://cloud.google.com/apis/docs/client-libraries-explained

**Table of contents:**

* [Quickstart](#quickstart)
  * [Before you begin](#before-you-begin)
  * [Installing the client library](#installing-the-client-library)
  * [Using the client library](#using-the-client-library)
* [Samples](#samples)
* [Versioning](#versioning)
* [Contributing](#contributing)
* [License](#license)

## Quickstart

### Before you begin

1.  Select or create a Cloud Platform project.

    [Go to the projects page][projects]

1.  Enable billing for your project.

    [Enable billing][billing]

1.  Enable the Google Cloud Data Loss Prevention (DLP) API API.

    [Enable the API][enable_api]

1.  [Set up authentication with a service account][auth] so you can access the
    API from your local workstation.

[projects]: https://console.cloud.google.com/project
[billing]: https://support.google.com/cloud/answer/6293499#enable-billing
[enable_api]: https://console.cloud.google.com/flows/enableapi?apiid=dlp.googleapis.com
[auth]: https://cloud.google.com/docs/authentication/getting-started

### Installing the client library

    npm install --save @google-cloud/dlp

### Using the client library

```javascript
// Imports the Google Cloud Data Loss Prevention library
const DLP = require('@google-cloud/dlp');

// Instantiates a client
const dlp = new DLP.DlpServiceClient();

// The string to inspect
const string = 'Robert Frost';

// The minimum likelihood required before returning a match
const minLikelihood = 'LIKELIHOOD_UNSPECIFIED';

// The maximum number of findings to report (0 = server maximum)
const maxFindings = 0;

// The infoTypes of information to match
const infoTypes = [
  { name: 'US_MALE_NAME' },
  { name: 'US_FEMALE_NAME' }
];

// Whether to include the matching string
const includeQuote = true;

// Construct items to inspect
const items = [{ type: 'text/plain', value: string }];

// Construct request
const request = {
  inspectConfig: {
    infoTypes: infoTypes,
    minLikelihood: minLikelihood,
    maxFindings: maxFindings,
    includeQuote: includeQuote
  },
  items: items
};

// Run request
dlp.inspectContent(request)
  .then((response) => {
    const findings = response[0].results[0].findings;
    if (findings.length > 0) {
      console.log(`Findings:`);
      findings.forEach((finding) => {
        if (includeQuote) {
          console.log(`\tQuote: ${finding.quote}`);
        }
        console.log(`\tInfo type: ${finding.infoType.name}`);
        console.log(`\tLikelihood: ${finding.likelihood}`);
      });
    } else {
      console.log(`No findings.`);
    }
  })
  .catch((err) => {
    console.error(`Error in inspectString: ${err.message || err}`);
  });
```

## Samples

Samples are in the [`samples/`](https://github.com/GoogleCloudPlatform/google-cloud-node/blob/master/samples) directory. The samples' `README.md`
has instructions for running the samples.

| Sample                      | Source Code                       |
| --------------------------- | --------------------------------- |
| Inspect | [source code](https://github.com/GoogleCloudPlatform/google-cloud-node/blob/master/samples/inspect.js) |
| Redact | [source code](https://github.com/GoogleCloudPlatform/google-cloud-node/blob/master/samples/redact.js) |
| Metadata | [source code](https://github.com/GoogleCloudPlatform/google-cloud-node/blob/master/samples/metadata.js) |
| DeID | [source code](https://github.com/GoogleCloudPlatform/google-cloud-node/blob/master/samples/deid.js) |
| Risk Analysis | [source code](https://github.com/GoogleCloudPlatform/google-cloud-node/blob/master/samples/risk.js) |

The [Data Loss Prevention (DLP) API Node.js Client API Reference][client-docs] documentation
also contains samples.

## Versioning

This library follows [Semantic Versioning](http://semver.org/).

This library is considered to be in **beta**. This means it is expected to be
mostly stable while we work toward a general availability release; however,
complete stability is not guaranteed. We will address issues and requests
against beta libraries with a high priority.

More Information: [Google Cloud Platform Launch Stages][launch_stages]

[launch_stages]: https://cloud.google.com/terms/launch-stages

## Contributing

Contributions welcome! See the [Contributing Guide](.github/CONTRIBUTING.md).

## License

Apache Version 2.0

See [LICENSE](LICENSE)

[client-docs]: https://cloud.google.com/nodejs/docs/reference/dlp/latest/
[product-docs]: https://cloud.google.com/dlp/docs/
