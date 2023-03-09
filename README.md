# kiwitcms-uploader-action

This action uses the python junit.xml plugin that uploads data to a kiwitcms instance.

The action wraps the install and execution of the uploader to make it fast to integrate in the element workflows.

Example configuration:

```
jobs:
   upload:
      name: Upload
      runs-on: ubuntu-latest
      steps:
         - uses: actions/checkout@v3
         - name: Activate plugin
           uses: vector-im/kiwitcms-upload-action@v1
           with:
              file-pattern: ./junit-files/*.xml
              kiwi-username: ${{ secrets.TCMS_USERNAME }}
              kiwi-password: ${{ secrets.TCMS_PASSWORD }}
              product: "Element Web"
              product-version: "main"
              build-id: ${{ github.sha }}
              suite-name: "Cypress E2E"
              summary-template: '$name'
```

File-pattern should link to one or many junit files. All files will be read and combined into one test run, making it ideal for post-processing parallelised test runs.

The summary-template option allows us to choose between $name, $classname and $suitename or a combination of them; depending on how the junit.xml is published.

The product should be a exact text match for existing products in the kiwi database - try not to create a new one ad-hoc without checking with QA team.

The product-version should be a fairly low-cardinality option, we create a test plan for each product + product-version. The build-id should be high-cardinality; ideally the release version for releases / release-candidates, or a simple SHA hash for commits. (TODO: evaluate exactly how this should be structured to fit in with other requirements). 

The suite-name should identify this class of tests from other classes of tests; for instance "Cypress E2E" vs "Unit tests" vs "Manual tests"

Finally the username/password are available from the kiwi configuration on the site.


