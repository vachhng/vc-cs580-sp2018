/* Veronica Chhang y299w467 */

#include <iostream>
#include <pthread.h>
#include <semaphore.h>
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include "pthread/pthread.h"
#define BUFFER_SIZE 5
using namespace std;

pthread_mutex_t mutex1;
pthread_t tid;
pthread_t attr;
sem_t full, empty;
typedef int buffer_item;
buffer_item buffer[BUFFER_SIZE];
int counter;

void *producer(void *param);
void *consumer(void *param);

void initialize() {
    pthread_mutex_init(&mutex1, NULL);
    sem_init(&full, 0, 0);
    sem_init(&empty, 0, BUFFER_SIZE);
    pthread_attr_init(&attr);
    counter = 0;
}

int insert_item(buffer_item item) {
    /* called by producer threads */
    if (counter < BUFFER_SIZE) {
        buffer[counter] = item;
        counter++;
        return 0;
    }
    else { return -1; }
}

int remove_item(buffer_item *item) {
    /* called by consumer threads */
    if (counter > 0) {
        *item = buffer[(counter-1)];
        counter--;
        return 0;
    }
    else { return -1; }
}

void *producer(void *param) {
    buffer_item item;
    while (true) {
        int random_num = rand()%10;
        /* sleep for a random period of time */
        sleep(random_num);
        /* generate a random number */
        item = rand();
        
        sem_wait(&empty);
        pthread_mutex_lock(&mutex1);
        
        if (insert_item(item))
            fprintf(stderr, "report error condition");
        else
            printf("producer produced %d\n", item);
        
        pthread_mutex_unlock(&mutex1);
        sem_post(&full);
    }
}
        
void *consumer(void *param) {
    buffer_item item;
    while (true) {
        int random_num = rand()%10;
        /* sleep for a random period of time */
        sleep(random_num);
        
        sem_wait(&full);
        pthread_mutex_lock(&mutex1);
        
        if (remove_item(&item))
            fprintf(stderr, "report error condition");
        else
            printf("consumer consumed %d\n",item);
        
        pthread_mutex_unlock(&mutex1);
        sem_post(&empty);
    }
}
                    
int main(int argc, char *argv[]) {
    //bounded-buffer problem using producer and consumer processes (figures 5.9/5.10)
    //use standard counting semaphores for empty and full and a mutex lock, rather than a binary semphore
    //run producer and consumer as separate threads
        //this will move items to and from a buffer thats synced with the empty, full, and mutex structs
    //can use pthreads or windows api

    int index;
    if (argc != 4)
        fprintf(stderr, "3 parameters needed");
    
    int sleep_time = atoi(argv[1]);
    int producer_threads = atoi(argv[2]);
    int consumer_threads = atoi(argv[3]);
    
    initialize();
    
    /* create producer thread */
    for (int i = 0; i < producer_threads; i++)
        pthread_create(&tid, &attr, producer, NULL);
    
    for (int i = 0; i < consumer_threads; i++)
        pthread_create(&tid, &attr, consumer, NULL);
        
    sleep(sleep_time);
    printf("Bye!");

    return 0;
}
