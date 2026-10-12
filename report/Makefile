# Report helper. Run from report/:  make | make watch | make summary | make package TEAM=Team_LDAP1_LDAP2
# (The repo-root Makefile builds ChampSim; this one only handles the report.)

DOC      = report
LATEXMK  = latexmk -pdf -interaction=nonstopmode -halt-on-error
# Figures live outside report/, so rebuild when any of them change.
FIGS     = $(wildcard ../results/*/plots/*.png)
TEAM    ?= TeamName_LDAP1_LDAP2
ROOT     = $(abspath ..)
REPO     = $(notdir $(ROOT))

.PHONY: all watch summary package clean distclean help

all: $(DOC).pdf

$(DOC).pdf: $(DOC).tex $(FIGS)
	$(LATEXMK) $(DOC).tex

# Rebuild automatically on every save (Ctrl-C to stop).
watch:
	latexmk -pdf -pvc -interaction=nonstopmode $(DOC).tex


summary: $(DOC).pdf
	cp $(DOC).pdf ../Summary.pdf
	@echo "Wrote ../Summary.pdf"


package: summary
	cd $(ROOT)/.. && tar --exclude='$(REPO)/traces' --exclude='$(REPO)/bin' \
	  --exclude='$(REPO)/obj' --exclude='$(REPO)/.git' \
	  -czf $(TEAM)_pa2.tar.gz $(REPO)
	@echo "Wrote $(ROOT)/../$(TEAM)_pa2.tar.gz"

# Remove LaTeX intermediates, keep report.pdf.
clean:
	latexmk -c $(DOC).tex
	$(RM) $(DOC).synctex.gz

distclean:
	latexmk -C $(DOC).tex
	$(RM) $(DOC).synctex.gz

help:
	@echo "make            build report.pdf (latexmk, reruns until refs settle)"
	@echo "make watch      live rebuild on save"
	@echo "make summary    copy report.pdf -> ../Summary.pdf"
	@echo "make package TEAM=...  build ../../<TEAM>_pa2.tar.gz for Moodle"
	@echo "make clean      delete aux files (keeps report.pdf)"
