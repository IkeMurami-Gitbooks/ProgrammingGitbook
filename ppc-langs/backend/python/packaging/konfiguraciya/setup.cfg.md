# setup.cfg

Считается **deprecated**, используем секцию в `pyproject.toml`.\
Конфиг для `setuptools` build backend.

Пример

```ini
[metadata]
name = asman
version = 0.0.1
description = The package with core functionality for Asman
long_description = file:README.md
long_description_content_type = text/markdown
author = Petrakov Oleg
author_email = test@gmail.com
url = https://github.com/asman-go/core
license = MIT
classifiers =
    Programming Language :: Python :: 3
    Programming Language :: Python :: 3.10
    Programming Language :: Python :: 3.11
    Programming Language :: Python :: 3.12
project_urls =
    Issue tracker = https://github.com/asman-go/core/issues

[options]
python_requires = >=3.10
install_requires =
    boto3
    grpcio
    grpcio-tools
    mock
    pydantic
    pydantic_settings
setup_requires =
    grpcio-tools
    setuptools
package_dir =
    asman = src
include_package_data = True

[options.package_data]
asman = py.typed

[options.extras_require]
dev =
    check-manifest
test =
    coverage

```
