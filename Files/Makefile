CC=gcc
CFLAGS=-g -o
TARGETS=writer_prefer fair_reader_writer
all: $(TARGETS)
.PHONY: all

%:
	$(CC) $(CFLAGS) $@ $@.c -lpthread

clean:
	rm $(TARGETS)
