//File Writing Using Parent and Child Processes.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <string.h>

int main() {
    // Open file BEFORE fork() so parent and child share the same file descriptor
    int fd = open("output.txt", O_CREAT | O_WRONLY | O_APPEND, 0644);
    if (fd < 0) {
        perror("File open failed");
        exit(1);
    }

    pid_t pid = fork();

    // ----------------
    // Write Child logic
    // ----------------
    if (pid == 0) {
        // Child process
        char *child_msg = "This line is written by the child process.\n";
        write(fd, child_msg, strlen(child_msg));
        close(fd);
        exit(0);  // Child exits after writing
    }

    // ----------------
    // Write Parent logic
    // ----------------
    else if (pid > 0) {
        // Parent waits for child to finish (prevents orphan process)
        wait(NULL);

        char *parent_msg = "This line is written by the parent process.\n";
        write(fd, parent_msg, strlen(parent_msg));
        close(fd);
    }

    else {
        // fork() failed
        perror("fork failed");
        close(fd);
        exit(1);
    }

    return 0;
}
