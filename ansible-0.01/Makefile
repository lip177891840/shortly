#!/usr/bin/make

ASCII2MAN = a2x -D $(dir $@) -d manpage -f manpage $<
ASCII2HTMLMAN = a2x -D docs/html/man/ -d manpage -f xhtml
MANPAGES := docs/man/man1/ansible.1
SITELIB = $(shell python -c "from distutils.sysconfig import get_python_lib; print get_python_lib()")

all: clean python

tests: 
	PYTHONPATH=./lib nosetests

docs: manuals

manuals: $(MANPAGES)

%.1: %.1.asciidoc
	$(ASCII2MAN)

%.5: %.5.asciidoc
	$(ASCII2MAN)

loc:
	sloccount lib library bin

pep8:
	@echo "#############################################"
	@echo "# Running PEP8 Compliance Tests"
	@echo "#############################################"
	pep8 -r --ignore=E501,E221,W291,W391,E302,E251,E203,W293,E231,E303,E201,E225 lib/ bin/

pyflakes:
	pyflakes lib/ansible/*.py

clean:
	@echo "Cleaning up distutils stuff"
	-rm -rf build
	-rm -rf dist
	@echo "Cleaning up byte compiled python stuff"
	find . -regex ".*\.py[co]$$" -delete
	@echo "Cleaning up editor backup files"
	find . -type f \( -name "*~" -or -name "#*" \) -delete
	find . -type f \( -name "*.swp" \) -delete
	@echo "Cleaning up asciidoc to man transformations and results"
	find ./docs/man -type f \( -name "*.xml" -or -regex ".*\.[0-9]$$" \) -delete
	@echo "Cleaning up output from test runs"
	-rm -rf test/test_data
	@echo "Cleaning up RPM stuff"
	-rm MANIFEST

python: docs
	python setup.py build

install: docs
	python setup.py install

rpm: 
	python setup.py sdist
	rpmbuild -ta dist/ansible-1.0.tar.gz

.PHONEY: docs manual clean pep8
vpath %.asciidoc docs/man/man1


