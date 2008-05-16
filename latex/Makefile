.SUFFIXES: .tex .dvi .dvi2 .ps .ps2 .pdf .bib .bbl

DVIDVI=dvidvi -m '2:0(3mm,-14mm),1(193mm,-14mm)'
DVIPS=dvips -Ppdf
DVI2PS=dvips -t landscape -x 707
PS2PDF=ps2pdf -dEmbedAllFonts#true

SRC=relato-v09
BIB=biblio

relato:	$(SRC).pdf

#pdflatex prefered here over latex
#use PNG and PDF for figures

$(SRC).pdf: $(SRC).tex dedica.tex resumo.tex agradece.tex \
	    chapter1.tex chapter2.tex chapter3.tex chapter4.tex chapter5.tex chapter6.tex chapter7.tex \
	    anexo1.tex anexo2.tex

.tex.pdf:
	rm -f $(SRC).pdf
	pdflatex $< $@ || { rm -f $*.aux $*.idx && false ; }
	if grep 'There were undefined references' $*.log ; then \
	  bibtex $(SRC); pdflatex $< $@; fi
	while grep 'Rerun to get cross-references right.' $*.log ; do \
	  pdflatex $< $@ || { rm -f $*.aux $*.idx && false ; } ; done

biblio:  $(BIB).bbl

.bib.bbl:
	bibtex $(SRC)

.tex.dvi:
	rm -f $*.ps $*.pdf
	latex $< || { rm -f $*.dvi $*.aux $*.idx && false ; }
	if grep 'There were undefined references' $*.log ; then \
	  bibtex $(SRC); latex $< $@; fi
	while grep 'Rerun to get cross-references right.' $*.log ; do \
	  latex $< || { rm -f $*.dvi $*.aux $*.idx && false ; } ; done

.dvi.ps:
	$(DVIPS) $< -o $@

.dvi.dvi2:
	$(DVIDVI) $< $@

.dvi2.ps2:
	$(DVI2PS) $< -o $@

.ps.pdf:
	$(PS2PDF) $< $@

$(SRC).ps:  	$(SRC).dvi
$(SRC).ps2:     $(SRC).dvi2
$(SRC).dvi2:	$(SRC).dvi
$(SRC).dvi:	$(SRC).tex $(BIB).bbl

clean:
	rm -f $(SRC){.aux,.nav,.log,.pdf,.toc,.snm,.out,.dvi,.ps} *.aux
