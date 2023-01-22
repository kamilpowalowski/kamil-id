---
Date: 2021-07-24 18:07
Location: /swiftwombat/how-to-setup-code-climate-quality-test-coverage-with-bitrise
Tags: Swift Wombat, Swift
---

# How to setup Code Climate Quality test coverage with Bitrise

![How to setup Code Climate Quality test coverage with Bitrise](/weblog/swiftwombat/covers/how_to_setup_code_climate_quality_code_coverage_with_bitrise.png)

[Quality by Code Climate](https://codeclimate.com/quality/) is a web service that gives you analytics for your code. It analyzes your code for code smells, and with proper CI/CD integration, it can track changes in test coverage data.

This article will list, step by step, how to set up Code Climate test coverage upload from [Bitrise](https://www.bitrise.io/) - probably most famous and widely used mobile Continues Integration and Continues Delivery service.

**Initial assumption**: Your GitHub repository is added to the Quality by Code Climate and the Bitrise.

- Get Code Climate's test reporters ID - visit `Repo Settings` on Code Climate and open `Test coverage`.
- Add ID from the previous step as a Bitrise Secret with a name `CC_TEST_REPORTER_ID` (if you want to test Pull Requests active `Expose for Pull Requests?`)
- Generate [GitHub personal access token](https://github.com/settings/tokens) (only `public_repo` permission is needed)
- Add token from the previous step as a Bitrise Secret with a name `GITHUB_ACCESS_TOKEN` (if you want to test Pull Requests active `Expose for Pull Requests?`)
![Bitrise secrets](/weblog/swiftwombat/images/33/bitrise_secrets.png)

- Turn on gathering test coverage in your Xcode project. `Edit Scheme...` -> `Test` phase and select checkbox on `Gather coverage for...` in `Code Coverage` section.
![Xcode - gather coverage](/weblog/swiftwombat/images/33/xcode_test_coverage.png)

- In Bitrise, go to your Tests workflow, and before `Xcode Test for iOS` step, add a new Script phase with content visible below. It will download Code Climate test runner (`cc-test-reporter`) and updates Code Climate that test coverage is being prepared.

```bash
#!/usr/bin/env bash

curl -L https://codeclimate.com/downloads/test-reporter/test-reporter-latest-darwin-amd64 > ./cc-test-reporter
chmod +x ./cc-test-reporter
./cc-test-reporter before-build
```

- Add a new step after `Xcode Test for iOS` and paste provided content. This script uses `xcrun xccov` to format `Tests.xcresult` file to JSON and uploads it using the previously downloaded `cc-test-reporter` script.

```bash
#!/usr/bin/env bash

xcrun xccov view --report $BITRISE_XCRESULT_PATH --json > coverage.json
./cc-test-reporter after-build --coverage-input-type xccov
```

![Bitrise steps needed for test coverage setup](/weblog/swiftwombat/images/33/bitrise_steps.png)

- In Xcode Test for iOS step set `Generate code coverage files`? to `no`. Note: I'm not sure why, but I had a problem finishing that step with this set to `yes`.

That's it. With that few simple steps, you can send test coverage data from Bitrise to Code Climate.
