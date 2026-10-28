# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them

```
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>
#include <stdlib.h>

struct message {
    long msg_type;
    char msg_text[100];
};

int main() {
    key_t key;
    int msgid;
    struct message msg;

    // Generate unique key
    key = ftok(".", 65);

    // Create message queue
    msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    // Take input from user
    printf("Enter message: ");
    fgets(msg.msg_text, sizeof(msg.msg_text), stdin);
    msg.msg_type = 1;

    // Send message
    if (msgsnd(msgid, &msg, sizeof(msg.msg_text), 0) == -1) {
        perror("msgsnd");
        return 1;
    }

    // Receive message
    if (msgrcv(msgid, &msg, sizeof(msg.msg_text), 1, 0) == -1) {
        perror("msgrcv");
        return 1;
    }

    // Display received message
    printf("Received Message: %s\n", msg.msg_text);

    // Remove message queue
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}

```



## OUTPUT

<img width="807" height="157" alt="image" src="https://github.com/user-attachments/assets/aaa62e29-39fa-4073-a44b-c84820e00dc2" />



# RESULT:
The programs are executed successfully.
