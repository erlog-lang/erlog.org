HTML = $(patsubst %.asciidoc,%.html,$(shell find . -depth 1 -name \*.asciidoc))
PDF = $(patsubst %.asciidoc,%.pdf,$(shell find . -depth 1 -name \*.asciidoc))

all: html

html: $(HTML)

pdf: $(PDF)

gh-pages: clean html
	git submodule update --init --recursive
	git clone -b gh-pages ../ gh-pages
	rsync -qavz ./*.html gh-pages/
	rsync -qavz --exclude=.git --delete ./bootstrap-docs/bootstrap/ gh-pages/bootstrap/
	rsync -qavz --exclude=.git --delete ./images/ gh-pages/images/
	perl -i -pe 's~//\@ sourceMappingURL=jquery.min.map~~;' gh-pages/bootstrap/docs/assets/js/jquery.js

clean:
	rm -rf $(HTML) $(PDF) gh-pages

%.html: %.asciidoc Makefile
	cd $(<D) && asciidoc $(<F)
	@perl -i -pe 's~class="brand" href="#"~class="brand" href="https://github.com/the-concurrent-schemer"~;' $(@)
	@perl -i -pe 's~rel="shortcut icon" href="./bootstrap/docs/assets/ico/favicon.ico"~rel="shortcut icon" href="http://www.gravatar.com/avatar/4b50e81570fd42f2d9a43c87f8c19456.png"~;' $(@)

%.pdf: %.asciidoc Makefile
	cd $(<D) && a2x -f pdf $(<F)

.PHONY: all html pdf clean
