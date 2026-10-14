# Report helper. Run from report/:  make | make watch | make summary | make package TEAM=Team_LDAP1_LDAP2
# (The repo-root Makefile builds ChampSim; this one only handles the report.)

DOC      = report
LATEXMK  = latexmk -pdf -interaction=nonstopmode -halt-on-error
# Figures live outside report/, so rebuild when any of them change.
FIGS     = $(wildcard ../results/*/plots/*.png)
TEAM    ?= "Arch-enemies of Latency_24B1021_24B1015_24B1037_24B0995"
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


# Moodle tarball: repo without traces, binaries, objects, git history, local backups and notes.
# Run as: make package TEAM=TeamName_LDAP1_LDAP2
package: summary
	cd $(ROOT)/.. && tar --exclude='$(REPO)/traces' --exclude='$(REPO)/bin' \
	  --exclude='$(REPO)/obj' --exclude='$(REPO)/.git' \
	  --exclude='$(REPO)/temp' --exclude='$(REPO)/PLAN.md' \
	  --exclude='__pycache__' --exclude='*.pyc' --exclude='*.part' \
	  --exclude='$(REPO)/report/*.aux' --exclude='$(REPO)/report/*.log' \
	  --exclude='$(REPO)/report/*.out' --exclude='$(REPO)/report/*.fls' \
	  --exclude='$(REPO)/scripts' \
	  --exclude='$(REPO)/report/*.fdb_latexmk' --exclude='$(REPO)/report/*.synctex.gz' \
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
