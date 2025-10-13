<div align="center">

# Reusable GitHub Actions

[![License](https://img.shields.io/github/license/jackd248/reusable-github-actions)](LICENSE.md)
[![Workflows](https://img.shields.io/badge/workflows-5-green)]()

</div>

This repository provides useful GitHub Action workflows.

> [!IMPORTANT]
> This package is intended for use in my personal projects only. It is not designed for general use.

## 🧩 Workflows

- [CGL](#cgl)
- [Tests](#tests)
- [Tests TYPO3](#tests-typo3)
- [Release](#release)
- [Release TYPO3](#release-typo3)

## CGL

Comprehensive code quality workflow that validates composer dependencies, runs linting (PHP, composer.json, editorconfig), performs static code analysis and checks rector migrations.

```yaml
name: CGL
on:
  push:
    branches:
      - '**'

jobs:
    cgl:
        uses: jackd248/reusable-github-actions/.github/workflows/cgl.yml@main
```

Input|Type| Required |Description
-|-|----------|-
`php-version`|input| false    |PHP version to use for the CGL check. Defaults to `8.3`.

## Tests

Matrix testing workflow that runs tests across multiple PHP versions with both highest and lowest dependencies. Includes optional coverage reporting to CodeClimate and Coveralls.

```yaml
name: Tests
on:
  push:
    branches:
      - '**'
        

jobs:
    tests:
        uses: jackd248/reusable-github-actions/.github/workflows/tests-php.yml@main
``` 

Input|Type| Required |Description
-|-|----------|-
`php-versions`|input| false    |PHP versions as JSON array. Defaults to `["8.2", "8.3", "8.4"]`.
`dependencies`|input| false    |Dependencies as JSON array. Defaults to `["highest", "lowest"]`.

## Tests TYPO3

Matrix testing workflow that runs tests across multiple PHP and TYPO3 versions with both highest and lowest dependencies. Includes optional coverage reporting to CodeClimate and Coveralls.

```yaml
name: Tests

on:
  push:
    branches:
      - '**'

jobs:
    tests:
        uses: jackd248/reusable-github-actions/.github/workflows/tests-typo3.yml@main
```

Input|Type| Required |Description
-|-|----------|-
`php-versions`|input| false    |PHP versions as JSON array. Defaults to `["8.2", "8.3", "8.4"]`.
`typo3-versions`|input| false    |TYPO3 versions as JSON array. Defaults to `["11.5", "12.4", "13.4"]`.
`dependencies`|input| false    |Dependencies as JSON array. Defaults to `["highest", "lowest"]`.

```yaml
name: Tests

on:
  push:
    branches:
      - '**'

jobs:
    tests:
        uses: jackd248/reusable-github-actions/.github/workflows/tests-typo3.yml@main
        with:
            php-versions: '["8.2", "8.3", "8.4"]'
            typo3-versions: '["11.5", "12.4", "13.4"]'
            dependencies: '["highest", "lowest"]'
```

### Optional Coverage Reporting

The Tests workflow includes optional coverage reporting to external services:

- **CodeClimate**: Set `CC_TEST_REPORTER_ID` secret to enable

If these secrets are not configured, the coverage steps will be skipped without causing workflow failures.

## Release

Automated release workflow that creates a new release on GitHub.

```yaml
name: Release

on:
  push:
    tags:
      - '*'

jobs:
  release:
        uses: jackd248/reusable-github-actions/.github/workflows/release.yml@main
```

## Release TYPO3

Automated release workflow for TYPO3 extension that creates a new release on GitHub and upload the extension artifact to the TER.

```yaml
name: Release

on:
  push:
    tags:
      - '*'

jobs:
  release:
        uses: jackd248/reusable-github-actions/.github/workflows/release-typo3.yml@main
        secrets:
          typo3-api-token: ${{ secrets.TYPO3_API_TOKEN }}
        with:
          typo3-extension-key: 'your_extension_key'
```

Input|Type| Required |Description
-|-|----------|-
`typo3-extension-key`|input| true    |TYPO3 extension key (as used in TER and GitHub repository name).
`typo3-api-token`|secret| true    |TYPO3 API token with permission to upload to TER.

## ⭐ License

This project is licensed under [GNU General Public License 3.0 (or later)](LICENSE).
