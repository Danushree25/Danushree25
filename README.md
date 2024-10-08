
#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *next;
    struct node *prev;
};

struct node *head = NULL;

void create();
void display();
void insert_begin();
void insert_end();
void insert_pos();
void delete_begin();
void delete_end();
void delete_pos();
struct node* getnode();

int main() {
    int choice;
    while (1) {
        printf("\nMenu");
        printf("\n1. Create");
        printf("\n2. Display");
        printf("\n3. Insert Begin");
        printf("\n4. Insert End");
        printf("\n5. Insert Position");
        printf("\n6. Delete Begin");
        printf("\n7. Delete End");
        printf("\n8. Delete Position");
        printf("\n9. Exit");
        printf("\nEnter choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: create(); break;
            case 2: display(); break;
            case 3: insert_begin(); break;
            case 4: insert_end(); break;
            case 5: insert_pos(); break;
            case 6: delete_begin(); break;
            case 7: delete_end(); break;
            case 8: delete_pos(); break;
            case 9: exit(0); break;
            default: printf("\nWrong choice"); break;
        }
    }
    return 0;
}

struct node* getnode() {
    struct node *newnode = (struct node *)malloc(sizeof(struct node));
    if (!newnode) {
        printf("Memory allocation failed\n");
        exit(1);
    }
    int value;
    printf("Enter value: ");
    scanf("%d", &value);
    newnode->data = value;
    newnode->next = NULL;
    newnode->prev = NULL;
    return newnode;
}

void create() {
    head = getnode();
}

void display() {
    if (head == NULL) {
        printf("List is empty\n");
    } else {
        struct node *temp = head;
        while (temp != NULL) {
            printf("%d ", temp->data);
            temp = temp->next;
        }
        printf("\n");
    }
}

void insert_begin() {
    struct node *newnode = getnode();
    if (head == NULL) {
        head = newnode;
    } else {
        newnode->next = head;
        head->prev = newnode;
        head = newnode;
    }
}

void insert_end() {
    struct node *newnode = getnode();
    if (head == NULL) {
        head = newnode;
    } else {
        struct node *temp = head;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        temp->next = newnode;
        newnode->prev = temp;
    }
}

void insert_pos() {
    struct node *newnode = getnode();
    if (head == NULL) {
        head = newnode;
    } else {
        int pos;
        printf("Enter position: ");
        scanf("%d", &pos);
        if (pos == 1) {
            insert_begin();
            return;
        }
        struct node *temp = head;
        for (int i = 1; temp != NULL && i < pos - 1; i++) {
            temp = temp->next;
        }
        if (temp == NULL) {
            printf("Position is out of bounds\n");
            free(newnode);
        } else {
            newnode->next = temp->next;
            newnode->prev = temp;
            if (temp->next != NULL) {
                temp->next->prev = newnode;
            }
            temp->next = newnode;
        }
    }
}

void delete_begin() {
    if (head == NULL) {
        printf("List is empty\n");
    } else {
        struct node *temp = head;
        head = head->next;
        if (head != NULL) {
            head->prev = NULL;
        }
        free(temp);
    }
}

void delete_end() {
    if (head == NULL) {
        printf("List is empty\n");
    } else {
        struct node *temp = head;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        if (temp->prev != NULL) {
            temp->prev->next = NULL;
        } else {
            head = NULL; 
        }
        free(temp);
    }
}

void delete_pos() {
    if (head == NULL) {
        printf("List is empty\n");
    } else {
        int pos;
        printf("Enter position: ");
        scanf("%d", &pos);
        struct node *temp = head;
        if (pos == 1) {
            delete_begin();
            return;
        }
        for (int i = 1; temp != NULL && i < pos; i++) {
            temp = temp->next;
        }
        if (temp == NULL) {
            printf("Position is out of bounds\n");
        } else {
            if (temp->prev != NULL) {
                temp->prev->next = temp->next;
            }
            if (temp->next != NULL) 
            {
                temp->next->prev = temp->prev;
            }
            free(temp);
        }
    }
}
