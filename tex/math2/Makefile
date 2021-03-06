LATEX=lualatex

LATEXOPT=--shell-escape --synctex=1
NONSTOP=--interaction=nonstopmode

LATEXMK=latexmk
LATEXMKOPT=-pdf
CONTINUOUS=-pvc

MAIN=main
SOURCES=$(MAIN).tex Makefile

all: once

.refresh:
	touch .refresh

force:
	touch .refresh
	rm -f $(MAIN).pdf
	$(LATEXMK) $(LATEXMKOPT) $(CONTINUOUS) \
		-pdflatex="$(LATEX) $(LATEXOPT) %O %S" $(MAIN)

clean:
	$(LATEXMK) -C $(MAIN)
	rm -f figures/*.dpth figures/*.md5 figures/*.pdf figures/*.log figures/*.run.xml

once: $(MAIN).tex .refresh $(SOURCES) $(FIGURES)
	./onfail.sh $(LATEXMK) $(LATEXMKOPT) -pdflatex='"$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S"' $(MAIN) \
		-- rubber-info $(MAIN)

continuous: $(MAIN).tex .refresh $(SOURCES) $(FIGURES)
	$(LATEXMK) $(LATEXMKOPT) $(CONTINUOUS) \
		-pdflatex="$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S" $(MAIN)

debug: once
	rubber-info $(MAIN)

lint:
	chktex -v0 $(shell find . -type f -name "*.tex"  -not -path "./template/*")

test: clean
	latexmk -pdf -pdflatex="echo X | lualatex --draftmode --shell-escape --interaction=errorstopmode %O %S \; touch %D" $(MAIN)

.PHONY: clean force once debug lint continuous test all
