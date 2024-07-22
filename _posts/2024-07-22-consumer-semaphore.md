---
layout:     post
title:      semaphore consumer Tagged Pointer
subtitle:   
date:       2024-07-22
author:    
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - exam
---
```


// consumer.c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <unistd.h>
// common.h
#include <semaphore.h>

#define FILE_PATH "shared.dat"
#define SEM_MUTEX_NAME "/semMutex"
#define SEM_EMPTY_NAME "/semEmpty"
#define SEM_FULL_NAME "/semFull"

int main() {
    int fd;
    int *numMessages;
    int status;

	sem_t *mutex, *empty, *full;
	
	// open shared memory
	fd = shm_open(FILE_PATH, O_RDWR, 0);
	// use fstatus get length of shared memory
	struct stat fstatus;
	status = fstat(fd, &fstatus);
	if (status == -1) {
		perror("fstat");
		exit(1);
	}
	// map shared memory
	numMessages = (int *)mmap(NULL, fstatus.st_size, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);

    status = close(fd);
	if (status == -1) {
		perror("close");
		exit(1);
	}

    mutex = sem_open(SEM_MUTEX_NAME, 0);
    if (mutex == SEM_FAILED) {
		perror("sem_open");
		exit(1);
    }

	empty = sem_open(SEM_EMPTY_NAME, 0);
    if (empty == SEM_FAILED) {
		perror("sem_open");
		exit(1);
    }

	full = sem_open(SEM_FULL_NAME, 0);
	if (full == SEM_FAILED) {
		perror("sem_open");
		exit(1);
	}

    while (1) {
        sem_wait(full); // indicate how many message can be consumed;
						// if full = 0, consumer will wait;
						// it will be increased by producer
        sem_wait(mutex);
        if (*numMessages > 0) {
            (*numMessages)--;
            printf("Consumed: %d\n", *numMessages);
        }
        sem_post(mutex);
        sem_post(empty); // indicate how many message can be produced;
						 // if empty = 0, producer will wait;
						 // so consumer will increase it
        sleep(1);  // Simulate work
    }

    return 0;
}

```
