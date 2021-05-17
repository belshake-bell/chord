TARGET = $(basename $(filter-out HEADER.tex,$(wildcard *.tex)))
SRC = $(addsuffix .tex,$(TARGET))
PDFTARGET = $(addsuffix .pdf,$(TARGET))
DVITARGET = $(addsuffix .dvi,$(TARGET))
MX2TARGET = $(addsuffix .mx2,$(TARGET))
BIBTARGET = $(addsuffix .bbl,$(TARGET))
MDXTARGET = $(addsuffix .ind,$(TARGET))
DVIPDFMxOpt = -f otf-up-yu-win10_mod #otf-up-sourcehan #
LOGSUFFIXES = .aux .log .toc .mx1 .mx2 .bcf .bbl .blg .idx .ind .ilg .out .run.xml
LATEXENGINE := uplatex
DVIWARE := dvipdfmx

define move
	$(foreach tempsuffix,$(LOGSUFFIXES),$(call movebase,$1,$(tempsuffix)))
	
endef
define movebase
	if [ -e $(addsuffix $2,$1) ]; then mv $(addsuffix $2,$1) ./logs; fi
	
endef


all: $(PDFTARGET)
muflx: $(MX2TARGET)
biblio: $(BIBTARGET)
makeindex: $(MDXTARGET)

.SUFFIXES: .pdf .dvi .tex .mx2 .mx1 .bbl .bcf .ind .idx

ronbun.dvi: ronbun.tex ./ronbun/*.tex
chord_rel.dvi: chord_rel.tex ./chord_rel/*.tex

%.dvi: %.tex
	uplatex $(notdir $<)
	if [ -e $(basename $(notdir $<)).mx1 ]; then $(MAKE) -B $(basename $(notdir $<)).mx2; uplatex $(notdir $<) ;fi
	if [ -e $(basename $(notdir $<)).bcf ]; then $(MAKE) -B $(basename $(notdir $<)).bbl; fi
	if [ -e $(basename $(notdir $<)).idx ]; then $(MAKE) -B $(basename $(notdir $<)).ind; fi
	uplatex $(notdir $<)
	uplatex -synctex=1 $(notdir $<)
	$(MAKE) movelog TARGET=$(basename $(notdir $<))

%.pdf: %.dvi
	dvipdfmx $(DVIPDFMxOpt) $(notdir $<)

%.mx2: %.mx1
	musixflx $(notdir $<)

%.bbl: %.bcf
	biber $(notdir $<)

%.ind: %.idx
	upmendex -s gcmc.ist -d dictU.dic -f $(notdir $<)

movelog:
	mkdir -p ./logs
	$(foreach temp,$(TARGET),$(call move,$(temp)))
#	if [ -e $(TARGET).aux ]; then mv $(TARGET).aux ./logs; fi
#	if [ -e $(TARGET).log ]; then mv $(TARGET).log ./logs; fi
#	if [ -e $(TARGET).toc ]; then mv $(TARGET).toc ./logs; fi
#	if [ -e $(TARGET).mx1 ]; then mv $(TARGET).mx1 ./logs; fi
#	if [ -e $(TARGET).mx2 ]; then mv $(TARGET).mx2 ./logs; fi
#	if [ -e $(TARGET).bcf ]; then mv $(TARGET).bcf ./logs; fi
#	if [ -e $(TARGET).bbl ]; then mv $(TARGET).bbl ./logs; fi
#	if [ -e $(TARGET).blg ]; then mv $(TARGET).blg ./logs; fi
#	if [ -e $(TARGET).idx ]; then mv $(TARGET).idx ./logs; fi
#	if [ -e $(TARGET).ind ]; then mv $(TARGET).ind ./logs; fi
#	if [ -e $(TARGET).ilg ]; then mv $(TARGET).ilg ./logs; fi
#	if [ -e $(TARGET).out ]; then mv $(TARGET).out ./logs; fi
#	if [ -e $(TARGET).run.xml ]; then mv $(TARGET).run.xml ./logs; fi

clean:
	rm -f $(DVITARGET)
	$(MAKE) movelog
