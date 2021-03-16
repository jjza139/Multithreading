# Multithreading

#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#define NUM_THREADS 5
int sum;

void *runner(void *param);

int main(int argc, char *argv[]){
    pthread_t workers[NUM_THREADS];
    pthread_attr_t attr[NUM_THREADS]; 
    if (argc > 6) {
        fprintf(stderr, "usage: multi.o <integer value>\n");
        return -1;
    }
    
    for(int i= 0;i< NUM_THREADS; i++){
        if (atoi(argv[i+1]) < 0) {
            fprintf(stderr, "%d must be >= 0\n", atoi(argv[i+1]));
        }
        else
        {
            printf("Runner[%d]\n",i);
            pthread_attr_init(&attr[i]); 
            pthread_create(&workers[i],&attr[i],runner,argv[i+1]);
            pthread_join(workers[i],NULL);
            printf("sum = %d\n", sum);
        }
    }
    
}
/* The thread will begin control in this function */ 
    void *runner(void *param){
        int i, upper = atoi (param); 
        sum = 0;
        for (i = 1; i <= upper; i++)
            sum += i;
        pthread_exit(0);
    }
