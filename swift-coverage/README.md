# SonarQube-Scanner for Swift Code Coverage

This example demonstrates how to import Xcode Coverage data (aka ProfData) to SonarQube for a Swift project.

## Prerequisites

* [SonarQube](http://www.sonarqube.org/downloads/) 8.9 LTS or Latest
* [SonarQube Scanner](http://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) 4.7+

## Usage

1. Run from "swift-coverage-example"

1.a Build project

```shell
xcodebuild -project swift-coverage-example.xcodeproj/ -scheme swift-coverage-example -derivedDataPath Build/ -enableCodeCoverage YES clean build test CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
```

1.b Create code coverage report

**Note** : The <device_id> used in the below command can be found from the XCode tools command: `instruments -s devices`

XCode version | Command
--- | ---
XCode 8+ - 9.2 | `xcrun llvm-cov show -instr-profile=Build/ProfileData/<device_id>/Coverage.profdata Build/Products/Debug/swift-coverage-example.app/Contents/MacOS/swift-coverage-example > Coverage.report`
XCode 9.3 - 9.4.1 | `bash xccov-to-sonarqube-generic.sh Build/Logs/Test/*.xccovarchive/ > sonarqube-generic-coverage.xml`
XCode 10 | `bash xccov-to-sonarqube-generic.sh Build/Logs/Test/*.xcresult/*_Test/*.xccovarchive/ > sonarqube-generic-coverage.xml`
XCode 11 & 12 | `bash xccov-to-sonarqube-generic.sh Build/Logs/Test/*.xcresult/ > sonarqube-generic-coverage.xml` <br> **Requires [jq](https://stedolan.github.io/jq/) (remove the optimize_format function use if you can't use it)**

1.c Import code coverage report

XCode version | Command
--- | ---
XCode 8.x - 9.2 | `sonar-scanner -Dsonar.projectKey=TestCoverage -Dsonar.sources=. -Dsonar.swift.coverage.reportPaths=Coverage.report`
XCode 9.3+ | `sonar-scanner -Dsonar.projectKey=TestCoverage -Dsonar.sources=. -Dsonar.coverageReportPaths=sonarqube-generic-coverage.xml`

2. Verify that for the project "swift-coverage-example" the coverage value is > 65%.
