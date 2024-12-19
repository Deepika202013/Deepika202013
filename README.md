/* 1. Fork and Print */
// forkprint.c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork failed");
        return 1;
    }
    
    if (pid == 0) {
        printf("I am child\n");
        return 0;
    } else {
        wait(NULL);  // Parent waits for child to finish
        printf("I am parent\n");
    }
    
    return 0;
}

/* 2. Fork and Print PID */
// forkpid.c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork failed");
        return 1;
    }
    
    if (pid == 0) {
        printf("Child PID: %d\n", getpid());
        return 0;
    } else {
        int status;
        waitpid(pid, &status, 0);
        printf("Parent: Child's PID was %d\n", pid);
    }
    
    return 0;
}

/* 3. Exec and Run ls */
// execls.c
#include <stdio.h>
#include <unistd.h>

int main() {
    char *args[] = {"ls", "-l", NULL};
    
    execv("/bin/ls", args);
    // If execv returns, it means it failed
    perror("execv failed");
    return 1;
}

/* 4. Run Command */
// runcmd.c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <command> <argument>\n", argv[0]);
        return 1;
    }
    
    pid_t pid = fork();
    
    if (pid < 0) {
        perror("fork failed");
        return 1;
    }
    
    if (pid == 0) {
        char *args[] = {argv[1], argv[2], NULL};
        execv(argv[1], args);
        perror("execv failed");
        return 1;
    } else {
        int status;
        wait(&status);
        if (WIFEXITED(status) && WEXITSTATUS(status) == 0) {
            printf("Command executed successfully\n");
        } else {
            printf("Command failed\n");
        }
    }
    
    return 0;
}

/* 5. Exec and Print Statements */
// execprint.c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("Before exec\n");
    fflush(stdout);  // Ensure the message is printed
    
    char *args[] = {"ls", NULL};
    execv("/bin/ls", args);
    
    printf("After exec (this won't print if exec succeeds)\n");
    return 0;
}

/* 6. Fork Multiple Times */
// forkmultiple.c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s <number_of_forks>\n", argv[0]);
        return 1;
    }
    
    int N = atoi(argv[1]);
    if (N < 1) {
        fprintf(stderr, "Please provide a positive number\n");
        return 1;
    }
    
    for (int i = 0; i < N; i++) {
        pid_t pid = fork();
        
        if (pid < 0) {
            perror("fork failed");
            return 1;
        }
        
        if (pid == 0) {
            printf("Child PID: %d\n", getpid());
            return 0;
        }
    }
    
    // Parent reaps all children
    for (int i = 0; i < N; i++) {
        wait(NULL);
    }
    
    return 0;
}

<!---
Deepika202013/Deepika202013 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
