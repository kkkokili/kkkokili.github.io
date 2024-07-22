---
layout:     post
title:      semaphore producer Tagged Pointer
subtitle:   
date:       2024-07-22
author:    
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - exam
---
` ` `
// producer.c
/*
 *Consider two processes: a producer and a consumer, and a shared variable numMessages. The shared variable starts with an initial value of 0. The producer increases the value of numMessages in steps of 1 in an infinite loop until it reaches a maximum value of 10. When the producer tries to increment the value of numMessages beyond 10, it must block until the value is less than 10. The consumer decrements the value of numMessages in steps of 1 in an infinite loop until it reaches a minimum value of 0. When the consumer tries to decrement the value of numMessages below 0, it must block until the value becomes positive. When the producer and consumer are run simultaneously, the following conditions must be satisfied:
 */
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
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
	
	// shm_open() creates a new shared memory object
	fd = shm_open(FILE_PATH, O_RDWR | O_CREAT, 0666);
	if (fd == -1) {
		perror("shm_open");
		exit(1);
	}

    status = ftruncate(fd, sizeof(int));
	if (status == -1) {
		perror("ftruncate");
		exit(1);
    }

	// shared variable numMessages
    numMessages = mmap(NULL, sizeof(int), PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    status = close(fd);  // No longer need the file descriptor
	if (status == -1) {
		perror("close");
		exit(1);
    }
	//binary semaphore for mutex
    mutex = sem_open(SEM_MUTEX_NAME, O_CREAT, 0666, 1);
	if (mutex == SEM_FAILED) {
		perror("sem_open");
		exit(1);
	}
	// numEmpty initialized to 10; since initially all the slots in the buffer are empty
    empty = sem_open(SEM_EMPTY_NAME, O_CREAT, 0666, 10);
	if (empty == SEM_FAILED) {
		perror("sem_open");
		exit(1);
	}

    // numFull initialized to 0; since initially none of the slots in the buffer is full
	full = sem_open(SEM_FULL_NAME, O_CREAT, 0666, 0);
	if (full == SEM_FAILED) {
		perror("sem_open");
		exit(1);
	}

    while (1) {
		// initial empty slots = 10; it's means producer can produce 10 messages
        sem_wait(empty); // slots - 1; // when it's 0, producer will wait; 
						 // consumer will increase the slots by sem_post(empty)
	
        sem_wait(mutex); // use for mutual exclusion; binary semaphore as mutex
        if (*numMessages < 10) {
            (*numMessages)++;
            printf("Produced: %d\n", *numMessages);
        }
        sem_post(mutex); // release the mutex

        sem_post(full); // indicate how many slots can be consumed; initially 0;
						// increase the slots by 1; consumer will consume the message
		
		//Todo random sleep time; max 1 sec
		sleep(1);
    }

    return 0;
}
` ` `
