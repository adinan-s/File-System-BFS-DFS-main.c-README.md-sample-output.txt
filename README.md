# File-System-BFS-DFS-main.c-README.md-sample-output.txt

#include <stdio.h>
#include <string.h>

#define MAX 20

char names[MAX][50];
int graph[MAX][MAX];
int visited[MAX];
int n, found;

int searchTarget;

int findNode(char target[]) {
    for (int i = 0; i < n; i++) {
        if (strcmp(names[i], target) == 0)
            return i;
    }
    return -1;
}

// Breadth First Search
void BFS(int start) {
    int queue[MAX], front = 0, rear = 0;
    int visited[MAX] = {0};
    found = 0;

    queue[rear++] = start;
    visited[start] = 1;

    printf("\nBFS Traversal: ");

    while (front < rear) {
        int current = queue[front++];

        printf("%s ", names[current]);

        if (current == searchTarget)
            found = 1;

        for (int i = 0; i < n; i++) {
            if (graph[current][i] == 1 && !visited[i]) {
                queue[rear++] = i;
                visited[i] = 1;
            }
        }
    }

    if (found)
        printf("\nFile found using BFS.\n");
    else
        printf("\nFile not found using BFS.\n");
}

// Depth First Search
void DFS(int current) {
    visited[current] = 1;

    printf("%s ", names[current]);

    if (current == searchTarget)
        found = 1;

    for (int i = 0; i < n; i++) {
        if (graph[current][i] == 1 && !visited[i]) {
            DFS(i);
        }
    }
}

int main() {
    char target[50];
    char startName[50];
    int start;

    printf("Enter number of nodes: ");
    scanf("%d", &n);

    printf("\nEnter file/directory names:\n");
    for (int i = 0; i < n; i++) {
        printf("Node %d: ", i);
        scanf("%49s", names[i]);
    }

    printf("\nEnter adjacency matrix:\n");
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &graph[i][j]);
        }
    }

    printf("\nEnter starting node: ");
    scanf("%49s", startName);

    start = findNode(startName);

    if (start == -1) {
        printf("Invalid starting node!\n");
        return 0;
    }

    printf("Enter file to search: ");
    scanf("%49s", target);

    searchTarget = findNode(target);

    if (searchTarget == -1) {
        printf("\nFile does not exist in the graph.\n");
        return 0;
    }

    BFS(start);

    for (int i = 0; i < n; i++)
        visited[i] = 0;

    found = 0;

    printf("\nDFS Traversal: ");
    DFS(start);

    if (found)
        printf("\nFile found using DFS.\n");
    else
        printf("\nFile not found using DFS.\n");

    return 0;
}
