# Nom du projet
NAME_PROJECT=main

#micro P1
PMC=-p1 -CCUSTOM
DLFLAGS :=-o1
OUTEXTEND=.binary
#micro P2
#PMC=-p2 -C P2_EDGE
#DLFLAGS :=-o2
#OUTEXTEND=.bin


# Compilateur
CC=catalina

#Baud
BAUD =230400

#PATH pour les includes catalina
INCLUDE="C:\Program Files (x86)\Catalina\include"

# Dossier build
BUILD_DIR=.\build

#Résultat de la compilation en .binary
OUTPUT_DIR=.\output

# Liste des fichiers .h
HFILES := $(wildcard *.h)

# Liste des fichiers .c
CFILES := $(wildcard *.c)

# Fichier .list
LSTFILES :=$(wildcard *.lst)

# Liste des fichiers en .obj
OBJFILES := $(patsubst %.c,$(BUILD_DIR)\\%.obj,$(CFILES))

# Flag de compilation par défaut (TTY)
CFLAGS = $(PMC) -CTTY -CTINY
LDFLAGS = $(PMC) -CTTY -CTINY -lci

# Creation des dossiers pour le projet
$(BUILD_DIR):
	mkdir $(BUILD_DIR)

$(OUTPUT_DIR):
	mkdir $(OUTPUT_DIR)
$(OBJFILES): |$(BUILD_DIR) $(OUTPUT_DIR)

#TTY
tty: build 
tty: dl-flash

# Flag de compilation pour TV
tv: CFLAGS =$(PMC) -CNTSC -CLORES_TV -CTINY -Clibci
tv: LDFLAGS =$(PMC) -CNTSC -CLORES_TV -CTINY -lci
tv: build

# Flag de compilation pour TV Res
tv-res: CFLAGS =$(PMC) -CNTSC -CHIRES_TV -CTINY -Clibci
tv-res: LDFLAGS =$(PMC) -CNTSC -CHIRES_TV -CTINY -lci
tv-res: build

# Flag de compilation pour VGA low
vga: CFLAGS =$(PMC) -CLORES_VGA -CTINY -Clibci
vga: LDFLAGS =$(PMC) -CLORES_VGA -CTINY -lci
vga: build
vga: dl

# Flag de compilation pour debug avec propterminal
term: CFLAGS =$(PMC) -CPROPTERMINAL -CTINY -Clibci
term: LDFLAGS =$(PMC) -CPROPTERMINAL -CTINY -lci
term: build

prop: term dl
	propterminal
# Flag de compilation pour debug
debug : CFLAGS =$(PMC) -g3 -CLORES_TV -CTINY -Clibci

# Flag de compilation pour debug avec l'interface graphique
debug-gui : CFLAGS =$(PMC) -g3 -CLORES_TV -CTINY -Clibci

#	Pre-compilation des fichiers .c en .obj
$(BUILD_DIR)\\%.obj : %.c $(CFILES) $(HFILES)
	$(CC) $(CFLAGS) -I$(INCLUDE) -c .\$< -o $@

build: $(OBJFILES)
	$(CC) -o output\$(NAME_PROJECT)$(OUTEXTEND) $(OBJFILES)  $(LDFLAGS)

dl:
	payload $(DLFLAGS) $(OUTPUT_DIR)\$(NAME_PROJECT)$(OUTEXTEND) -e

dl-ram:
	payload $(OUTPUT_DIR)\$(NAME_PROJECT)$(OUTEXTEND) -i -z -b$(BAUD) -q1
	@cls

dl-flash:
	flash_payload $(OUTPUT_DIR)\$(NAME_PROJECT)$(OUTEXTEND) $(DLFLAGS) -b$(BAUD)

debug: build
#@del $(wildcard *.debug) 
	@echo quit puis y pour quitter
	payload_blackbox.bat $(OUTPUT_DIR)\$(NAME_PROJECT)


debug-gui: build
	payload_blackcat.bat $(OUTPUT_DIR)\$(NAME_PROJECT)

# Nettoyage du dossier build
clear:
	@del /f /q 	$(BUILD_DIR)
	@del /f /q	$(OUTPUT_DIR)
