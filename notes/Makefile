.SUFFIXES: .tex .pdf .dia .eps
.PHONY: continuous default all clean distclean ott
SHELL:=/bin/bash

# Directories
ROOT := ..
TEX := tex
GEN := _gen
GEN_TEX := $(GEN)/$(TEX)
OTT := $(ROOT)/ott


# Files
OTTS := $(OTT)/*.ott
PDFS := main.pdf
TEXS := main.tex $(wildcard $(TEX)/*.tex) 
GEN_TEXS := $(addprefix $(GEN)/,$(TEXS))

OTT_GEN := $(OTT)/_gen
CACHE := $(OTT_GEN)/ott.cache

BIBLIO := $(ROOT)/bibliography.sty $(ROOT)/biblio.bib

all: $(PDFS)

# MAKE DIRECTORIES
$(GEN) $(GEN_TEX): %:
	mkdir -p $@

# MAKE OTT DEFITIONS

ott: 
	make -C $(OTT)

# MAKE PROCESSED TEX FILES
$(GEN_TEXS): ott $(ROOT)/prelude.tex | $(GEN_TEX)
	ott -generate_aux_rules false  -readsys $(CACHE) -tex_filter $(@:$(GEN)/%=%) $@

# MAIN

main.pdf: $(GEN_TEXS) $(BIBLIO)
	rubber --pdf -f $(GEN)/main.tex

continuous:
	make & fswatch -o .. | xargs -n1 -I{} make

# CLEAN
clean:
	rm -f *.aux *.lof *.log *.lot *.out *.toc *.nav *.snm *.bcf *.bbl *.blg *.xml *.tdo *.dvi
	rm -f *~ \#*\# *.spl *.fls *.fdb_latexmk *.bcf
	rm -f *.rubbercache 

ottdistclean:
	make -C $(OTT) distclean

distclean: clean ottdistclean
	rm -r $(GEN)
	rm -r $(OTT_GEN)
	rm $(PDFS)