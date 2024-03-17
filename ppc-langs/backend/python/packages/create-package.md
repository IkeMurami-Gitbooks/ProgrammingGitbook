# Create package

## setup.cfg

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

## setup.py

Пример

```python
from setuptools import setup, find_packages


__title__ = 'asman'
__description__ = 'The package with core functionality for Asman'
__url__ = 'https://github.com/asman-go/core'
__version__ = '0.0.1'
__author__ = 'Petrakov Oleg'
__author_email__ = 'test@gmail.com'
__license__ = 'MIT'

print(find_packages(where="src"))

# https://xebia.com/blog/a-practical-guide-to-using-setup-py/
setup(
    name=__title__,
    version=__version__,

    url=__url__,
    author=__author__,
    author_email=__author_email__,

    packages=find_packages(where="src"),
    package_dir={
        "": "src"
        # "asman.bbprograms": "src/bbprograms",
        # "asman.core": "src/core",
        # "asman.domains": "src/domains",
    },
    python_requires=">=3.10",
    install_requires=[
        'boto3',
        'grpcio',
        'grpcio-tools',
        'pydantic',
        'pydantic_settings',
    ],
    setup_requires=[
        'grpcio-tools',
        'setuptools',
    ],
    # tests_require=[
    #     'mock',
    #     'moto[all]',
    #     'pytest'
    # ],
    extras_require={
        'dev': [
            'check-manifest'
        ],
        'test': [
            'coverage'
        ]
    },
    license=__license__,
    description=__description__
)

```
