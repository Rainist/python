.PHONY: init
## init: install requirements
init:
{%- if cookiecutter.use_pipenv|lower != 'n' %}
	pip install pipenv
	pipenv install --dev
{%- else %}
	pip install -r requirements.txt
	pip install -r requirements-dev.txt
{%- endif %}

.PHONY: check
## check: check if everything's okay
check:
	isort --recursive --check-only {{ cookiecutter.package_name }} tests
{%- if cookiecutter.use_black|lower != 'n' %}
	black -S -l 79 --check {{ cookiecutter.package_name }} tests
{%- endif %}
	pylint {{ cookiecutter.package_name }}
{%- if cookiecutter.use_mypy|lower != 'do not use' %}
	mypy {{ cookiecutter.package_name }}
{%- endif %}

.PHONY: format
## format: format files
format:
	isort -rc -y {{ cookiecutter.package_name }} tests
{%- if cookiecutter.use_black|lower != 'n' %}
	black -S -l 79 {{ cookiecutter.package_name }} tests
{%- endif %}

.PHONY: test
## test: run tests
test:
	python -m pytest

.PHONY: coverage
## coverage: run tests with coverage
coverage:
	python -m pytest --cov {{ cookiecutter.package_name }} --cov-report term --cov-report xml

.PHONY: htmlcov
## htmlcov: run tests with coverage and create coverage report HTML files
htmlcov:
	python -m pytest --cov {{ cookiecutter.package_name }} --cov-report html
	rm -rf /tmp/htmlcov && mv htmlcov /tmp/
	open /tmp/htmlcov/index.html

{%- if cookiecutter.use_pipenv|lower != 'n' %}

.PHONY: requirements
## requirements: update requirements.txt
requirements:
	pipenv lock -r > requirements.txt
	pipenv lock -dr > requirements-dev.txt
{%- endif %}

.PHONY: help
## help: prints this help message
help:
	@echo "Usage: \n"
	@sed -n 's/^##//p' ${MAKEFILE_LIST} | column -t -s ':'
