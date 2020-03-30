# Artificial Intelligence
## Projects Repository
Created by:

Michael Ricky (05111840000078)

Sepuluh Nopember Institute of Technology

Surabaya, Indonesia

# 8-Puzzle
8-Puzzle adalah sebuah game yang dimana objective dari game tersebut adalah menggeser angka-angka yang ada sehingga menjadi suatu yang dinamakan "goal state".
Untuk mencapai goal state, pemain dapat menggerakkan angka-angka yang dapat bergerak sehingga state dari game boardnya berubah.
Dalam kasus 8-Puzzle Solver, movement mengacu pada gerakan angka "0" atau blank tile, yaitu tile yang kosong dari total 9 tile dalam game. AI berguna untuk mencari jalan sampai menuju goal state berdasarkan parameter-parameter yang telah di tetapkan sebelumnya. Ada beberapa metode penyelesaian untuk kasus 8-Puzzle, yaitu dengan metode Breadth-First Search (BFS), Depth-First Search (DFS), Iterative Deepening Search (IDS), dan A-Star (A*, Heuristic).

## Breadth-First Search (BFS)
Link ke file yang dibuat:
* [8puzzlebfs.c](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/BFS/8puzzlebfs.c) - 8-Puzzle Solver BFS

Breadth-First Search adalah pencarian "Brute Force" a.k.a. uninformed search dengan prioritas level terlebih dahulu, sehingga akan mengeksplor seluruh children dari 1 node tertentu dan neighbors dahulu baru dilanjutkan ke tingkat depth bawahnya.
```
solutionPath* BFS_search(State *init, State *goalState)
{
	NodeList *queue = NULL;
    NodeList *children = NULL;
    Node *node = NULL;

    clock_t start = clock();

    pushNode(createNode(0, init, NULL), &queue);
    Node *root = queue->head->currNode;

    while(queue->nodeCount > 0)
    {
        //pop the last node (tail) of the queue
        node = popNode(&queue);

        //if the state of the node is the goal state
        if(checkGoal(node->state, goalState))
        {
            break;
        }

        //else, expand the node and update the expanded-nodes counter
        children = getChildren(node, goalState);

        ++nodesExpanded;

        //add the node's children to the queue
        pushList(&children, queue);
    }

    runtime = (double)(clock() - start) / CLOCKS_PER_SEC;

    //get solution path in order from the root, if it exists
    solutionPath *pathHead = NULL;
    solutionPath *newPathNode = NULL;

    while(node)
    {
        newPathNode = malloc(sizeof(solutionPath));
        newPathNode->action = node->state->action;
        newPathNode->next = pathHead;
        pathHead = newPathNode;

        //update the solution length and move on the next node
        ++solutionLength;
        node = node->parent;
    }

    --solutionLength; //uncount the root node

    //deallocate the generated tree
    destroyTree(root);

    return pathHead;
}
```

Potongan kode diatas merupakan algoritma BFS yang telah di terapkan dalam program 8puzzlebfs. Algoritma dimulai dengan pembentukan queue sebagai main object di dalam algoritma ini. Queue tersebut berguna untuk menyimpan urutan node yang akan di eksplor terlebih dahulu. Jika ada node A, B, C, D, E, F, G dalam suatu BST, dan bentuk BST yang dimaksud seperti berikut :
```
                    A
                   / \
                  B   C
                 /\   /\
                D  E F  G
```

maka, algoritma akan dimulai dengan traversing tree dari root, yaitu A. Kemudian, anak A akan di list dan di masukkan ke dalam queue.
```
Queue1 : | B | C |   |   |   |
```

Dari queue tersebut lah yang menjadi node berikutnya yang akan di eksplor. Berarti, sekarang akan traverse ke B. Kembali lagi, seluruh anak B akan di masukkan ke dalam Queue, dan B di keluarkan dari queue karena sudah tereksplor.
```
Queue1 : | C | D | E |   |   |
```

Node C di eksplor, dan seluruh anaknya dimasukkan ke dalam queue. C dibuang dari queue.
```
Queue1 : | D | E | F | G |   |
```

D dieksplor. Karena D adalah leaves, maka tidak ada yang dimasukkan ke dalam queue, dan D dibuang dari queue.
```
Queue1 : | E | F | G |   |   |
```

E dieksplor. Karena E adalah leaves, maka tidak ada yang dimasukkan ke dalam queue, dan E dibuang dari queue.
```
Queue1 : | F | G |   |   |   |
```

F dieksplor. Karena F adalah leaves, maka tidak ada yang dimasukkan ke dalam queue, dan F dibuang dari queue.
```
Queue1 : | G |   |   |   |   |
```

F dieksplor. Karena F adalah leaves, maka tidak ada yang dimasukkan ke dalam queue, dan G dibuang dari queue.
```
Queue1 : |   |   |   |   |   |
```

Karena Queue1 sudah kosong, maka BFS tree traversal selesai.

Dengan logika yang serupa, pencarian jalan ke state yang disebut "goal state" menggunakan teknik yang sama. Namun, selesainya traversal ditandai dengan ditemukannya posisi node yang memiliki goal state. Setelah selesai dengan pencarian, maka akan di print movementnya sesuai yang telah di input tiap move.

Dalam program ini, goal state yang dimaksud adalah ```{1, 2, 3, 8, 0, 4, 7, 6, 5}```, tetapi user akan memasukkan initial state sendiri.

## Test Case
**Goal State**:
<table>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr>
    <td>8</td>
    <td>0</td>
    <td>4</td>
  </tr>
  <tr>
    <td>7</td>
    <td>6</td>
    <td>5</td>
  </tr>
</table>

**Test Case Init State**:
<table>
  <tr>
    <td>
      <table>
        <caption>Easy</caption>
        <tr>
          <td>1</td>
          <td>3</td>
          <td>4</td>
        </tr>
        <tr>
          <td>8</td>
          <td>6</td>
          <td>2</td>
        </tr>
        <tr>
          <td>7</td>
          <td>0</td>
          <td>5</td>
        </tr>
      </table>
    </td>
    <td>
      <table>
        <caption>Medium</caption>
        <tr>
          <td>2</td>
          <td>8</td>
          <td>1</td>
        </tr>
        <tr>
          <td>0</td>
          <td>4</td>
          <td>3</td>
        </tr>
        <tr>
          <td>7</td>
          <td>6</td>
          <td>5</td>
        </tr>
      </table>
    </td>
    <td>
      <table>
        <caption>Hard</caption>
        <tr>
          <td>2</td>
          <td>8</td>
          <td>1</td>
        </tr>
        <tr>
          <td>4</td>
          <td>6</td>
          <td>3</td>
        </tr>
        <tr>
          <td>7</td>
          <td>5</td>
          <td>0</td>
        </tr>
      </table>
    </td>
  </tr>
</table>

Berikut adalah hasil jalan program :
Easy :

![easy](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/BFS/images/image1.png)

Medium :

![medium](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/BFS/images/image2.png)

Hard :

![hard](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/BFS/images/image3.png)


## Depth-First Search (DFS)
Link ke file yang dibuat:
* [8puzzledfs.c](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/DFS/8puzzledfs.c) - 8-Puzzle Solver DFS

Depth-First Search adalah suatu teknik uninformed search yang dimana algoritma yang digunakan adalah kebalikan dari BFS. Dalam BFS, kita mencari node dengan level order, dimana kita akan mengeksplor node yang selevel terlebih dahulu, baru dilanjutkan dengan depth-level yang lebih dalam. Dalam DFS, kita akan mencari node dengan depth order, dimana kita akan mengeksplor node sampai paling dasar terlebih dahulu, lalu akan dilanjutkan dengan backtrack ke parent node. Untuk DFS, struktur data yang digunakan adalah stack.

Misal ada tree sebagai berikut :
```
                    A
                   / \
                  B   C
                 /\   /\
                D  E F  G
```

Traverse akan dimulai dari root node, A. Kemudian, children dari A akan di list dan dimasukkan kedalam stack.
```
Stack1 : |   |   |   | B | C |
```

Node B dieksplor, node B di pop dari stack, dan childrennya dimasukkan ke dalam stack.
```
Stack1 : |   |   | D | E | C |
```

Node D dieksplor, node D di pop dari stack. D adalah leaves, melanjutkan sequence.
```
Stack1 : |   |   |   | E | C |
```

Node E dieksplor, node E di pop dari stack. E adalah leaves, melanjutkan sequence.
```
Stack1 : |   |   |   |   | C |
```

Node C dieksplor, node C di pop dari stack, dan childrennya dimasukkan ke dalam stack.
```
Stack1 : |   |   |   | F | G |
```

Node F dieksplor, node F di pop dari stack. F adalah leaves, melanjutkan sequence.
```
Stack1 : |   |   |   |   | G |
```

Node G dieksplor, dan karena node G adalah leaves, maka selesailah sequence DFS.

Cara pencarian goal state persis dengan algoritma lainnya, yaitu dengan pencocokan dan pencarian path ke goal state yang ditemukan. Namun, dikarenakan tree state yang dibentuk tidak akan memiliki leaves (karena walau bagaimanapun, game akan dapat melanjutkan move), maka jika state bukan merupakan state yang hanya perlu 1 move untuk mencapai goal state, maka akan terjadi infinite loop.


## Iterative Deepening Search (IDS)
Link ke file yang dibuat:
* [8puzzleids.c](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/IDS/8puzzleids.c) - 8-Puzzle Solver IDS
--Untuk File Belum Ada--

## A-Star Search (A* S)
Link ke file yang dibuat:
* [8puzzleastar.c](https://github.com/djtyranix/AIProject/blob/master/8%20Puzzle/AStar/8puzzleastar.c) - 8-Puzzle Solver A*
--Untuk File Belum Ada--
