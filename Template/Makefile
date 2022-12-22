# Nom du projet
NAME_PROJECT=main

#micro P1
PMC=-p1 -CCUSTOM
DLFLAGS :=-o1
OUTEXTEND=.binary



# Compilateur
CC=catalina

#Baud
BAUD =115200

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
tty: dl

# Flag de compilation pour debug avec propterminal
term: CFLAGS =$(PMC) -CPROPTERMINAL -CTINY -Clibci
term: LDFLAGS =$(PMC) -CPROPTERMINAL -CTINY -lci
term: build

prop: term dl
	propterminal

#	Pre-compilation des fichiers .c en .obj
$(BUILD_DIR)\\%.obj : %.c $(CFILES) $(HFILES)
	$(CC) $(CFLAGS) -I$(INCLUDE) -c .\$< -o $@

build: $(OBJFILES)
	$(CC) -o output\$(NAME_PROJECT)$(OUTEXTEND) $(OBJFILES)  $(LDFLAGS)

dl:
	payload  $(OUTPUT_DIR)\$(NAME_PROJECT)$(OUTEXTEND) -e $(DLFLAGS)


# Nettoyage du dossier build
clear:
	@del /f /q 	$(BUILD_DIR)
	@del /f /q	$(OUTPUT_DIR)
