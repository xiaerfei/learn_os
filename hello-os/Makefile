# Viz @ 2021.05.15

TARGET := hello-os.img
OBJS := entry.o main.o vgastr.o

ASM := nasm
CC  := /usr/local/Cellar/x86_64-elf-gcc/10.2.0/bin/x86_64-elf-gcc
LD  := /usr/local/Cellar/x86_64-elf-binutils/2.35.1/bin/x86_64-elf-ld

ASMFLAGS := -f elf32
CFLAGS   := -c -Os -std=c99 -m32 -Wall -Wextra -Werror -fno-pie -fno-stack-protector -fomit-frame-pointer -fno-builtin -fno-common -ffreestanding
LDFLAGS  := -s -static -T layout.ld -n -m elf_i386

.PHONY: all clean run docker docker-image

all: clean run

terminal:
	qemu-system-x86_64 -fda hello-os.img -boot a

run: $(TARGET)
	qemu-system-x86_64 -curses -drive 'file=$<,format=raw,index=0,media=disk'
clean:
	rm -f $(TARGET) $(OBJS)

$(TARGET): $(OBJS)
	$(LD) $(LDFLAGS) -o $@ $^

%.o: %.asm
	$(ASM) $(ASMFLAGS) -o $@ $<
%.o: %.c *.h
	$(CC) $(CFLAGS) -o $@ $<

docker: docker-image
	docker run --rm -it --name hello-os-emu hello-os-emu

docker-image: Dockerfile
	docker build -t hello-os-emu .
