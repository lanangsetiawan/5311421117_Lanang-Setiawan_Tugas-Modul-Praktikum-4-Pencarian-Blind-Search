Nama  : Lanang Setiawan
NIM   : 5311421117

# Teknik Heuristic Search
# A. Tujuan Praktikum
Memperlihatkan kepada mahasiswa bagaimana menyelesaikan permasalahan pada game 8-puzzle  dengan menggunakan algoritma heuristic search. Mahasiswa diharapkan mampu  mengimplementasikan algoritma heuristic dengan menggunakan Java.
# B. Dasar teori
Heuristic search merupakan pendekatan dalam ilmu komputer dan kecerdasan buatan yang memanfaatkan informasi tambahan yang dikenal sebagai heuristik untuk mengeksplorasi ruang pencarian dengan lebih efisien. Dalam konteks ini, heuristik adalah estimasi yang memberikan informasi tentang sejauh mana sebuah keadaan mendekati tujuan dalam suatu masalah. Pencarian heuristik berbeda dari pencarian buta karena menggunakan pengetahuan tambahan ini untuk memprioritaskan cabang-cabang yang akan dieksplorasi dalam ruang pencarian. Algoritma A* adalah salah satu contoh algoritma pencarian heuristik yang populer, yang menggabungkan biaya aktual dengan heuristik untuk mencapai solusi yang lebih efisien. Meskipun pencarian heuristik memiliki kelebihan dalam efisiensi, harus diingat bahwa pilihan heuristik yang tepat sangat penting, dan mereka tidak selalu menghasilkan solusi optimal. Pencarian heuristik merupakan komponen kunci dalam pengembangan algoritma cerdas dan pemecahan masalah yang lebih efisien dalam berbagai konteks.
# C. Coding

    import java.util.Vector;

    class Node {
    int[] state = new int[9];
    int cost;
    Node parent = null;
    Vector<Node> successors = new Vector<Node>();

    Node(int s[], Node parent) {
        this.parent = parent;
        for (int i = 0; i < 9; i++)
            state[i] = s[i];
    }

    public Node() {
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < 9; i++) {
            s = s + state[i] + " ";
        }
        return s;
    }

    public boolean equals(Object node) {
        Node n = (Node) node;
        boolean result = true;
        for (int i = 0; i < 9; i++) {
            if (n.state[i] != state[i])
                result = false;
        }
        return result;
    }

    Vector<Node> getPath(Vector<Node> v) {
        v.insertElementAt(this, 0);
        if (parent != null)
            v = parent.getPath(v);
        return v;
    }

    Vector<Node> getPath() {
        return getPath(new Vector<Node>());
    }
    public static void main(String args[]) {
        new Node().run();
    }

    private void run() {
    }
    }

    class EightPuzzleSpace {
    Node getRoot() {
        int initialState[] ={3, 1, 2, 4, 7, 5, 6, 8, 0};
        return new Node(initialState, null);
    }

    Node getGoal() {
        int goalState[] = {1, 2, 3, 4, 0, 8, 5, 6, 7};
        return new Node(goalState, null);
    }

    Vector<Node> getSuccessors(Node parent) {
        Vector<Node> successors = new Vector<Node>();
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                if (parent.state[(r * 3) + c] == 0) {
                    if (r > 0) {
                        successors.add(transformState(r - 1, c, r, c, parent));
                    }
                    if (r < 2) {
                        successors.add(transformState(r + 1, c, r, c, parent));
                    }
                    if (c > 0) {
                        successors.add(transformState(r, c - 1, r, c, parent));
                    }
                    if (c < 2) {
                        successors.add(transformState(r, c + 1, r, c, parent));
                    }
                }
            }
        }
        parent.successors = successors;
        return successors;
    }

    Node transformState(int r0, int c0, int r1, int c1, Node parent) {
        int[] s = parent.state;
        int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
        newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
        newState[(r0 * 3) + c0] = 0;
        return new Node(newState, parent);
    }

    public static void main(String args[]) {
        new EightPuzzleSpace().run();
    }

    private void run() {
    }




    }

    class EightPuzzleSearch {
    EightPuzzleSpace space = new EightPuzzleSpace();
    Vector<Node> open = new Vector<Node>();
    Vector<Node> closed = new Vector<Node>();

    int h1Cost(Node node) {
        int cost = 0;
        for (int i = 0; i < node.state.length; i++) {
        if (node.state[i] != i) cost++; }
        return cost;
        }

    int h2Cost(Node node) {
        int cost = 0;
        int state[] = node.state;
        for (int i = 0; i < state.length; i++) {
            int v0 = i, v1 = state[i];
            if (v1 == 0)
                continue;
            int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
            int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
            cost += c;
        }
        return cost;
    }
    /*boleh diubah dengan memakai heuristic h1 atau h2 */
    int hCost(Node node) {
    return h1Cost(node);
    }
    Node getBestNode(Vector nodes) {
        int index = 0, minCost = Integer.MAX_VALUE;
        for (int i = 0; i < nodes.size(); i++) {
            Node node = (Node)nodes.elementAt(i);
            if (node.cost < minCost) {
                minCost = node.cost;
                index = i;
            }
        }
        Node bestNode = (Node)nodes.remove(index);
        return bestNode;
    }

    int getPreviousCost(Node node) {
        int i = open.indexOf(node);
        int cost = Integer.MAX_VALUE;
        if (i != -1) {
            cost = open.get(i).cost;
        } else {
            i = closed.indexOf(node);
            if (i != -1)
                cost = closed.get(i).cost;
        }
        return cost;
    }

    void printPath(Vector path) {
        for (int i = 0; i < path.size(); i++) {
            System.out.print(" " + path.elementAt(i) + "\n");
        }
    }

    void run() {
        Node root = space.getRoot();
        Node goal = space.getGoal();
        Node solution = null;
        open.add(root);
        System.out.print("\nRoot: " + root + "\n\n");
        while (open.size() > 0) {
            Node node = getBestNode(open);
            int pathLength = node.getPath().size();
            closed.add(node);
            if (node.equals(goal)) {
                solution = node;
                break;
            }
            Vector<Node> successors = space.getSuccessors(node);
            for (int i = 0; i < successors.size(); i++) {
                Node successor = successors.get(i);
                int cost = hCost(successor) + pathLength + 1;
                int previousCost;
                previousCost = getPreviousCost(successor);
                boolean inClosed;
                inClosed = closed.contains(successor);
                boolean inOpen = open.contains(successor);
                if (!(inClosed || inOpen) || cost < previousCost) {
                    if (inClosed)
                        closed.remove(successor);
                    if (!inOpen)
                        open.add(successor);
                    successor.cost = cost;
                    successor.parent = node;
                }
            }
        }
        if (solution != null) {
            Vector path = solution.getPath();
            System.out.print("\nSolution found\n");
            printPath(path);
        }
    }

    public static void main(String args[]) {
        new EightPuzzleSearch().run();
    }
    }




# D. Tugas 

1. Pelajari class EightPuzzleSearch, EightPuzzleSpace, dan Node.

2. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya  Gambar 8. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal  state. Analisa dan bedakan dengan solusi pada point 1.

3. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya Gambar 5.9. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai  goal state. Analisa dan bedakan dengan solusi pada point 1 dan 2.

4. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya  Gambar 5.10. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai  goal state. Analisa dan bedakan dengan solusi pada point 1, 2, dan 3.

5. Ubahlah initial dan goal state dari program dan class-class di atas sehingga bentuk initial dan  goal statenya Gambar 5.11. Kemudian tentukan langkah-langkah mana saja sehingga  puzzlenya mencapai goal state.

# E. Jawaban
1. Pelajari class EightPuzzleSearch, EightPuzzleSpace, dan Node.
Dalam kode yang diberikan, terdapat tiga kelas utama: Node, EightPuzzleSpace, dan EightPuzzleSearch. Mari kita bahas peran masing-masing kelas:
* Node:
Node merupakan suatu kelas yang digunakan untuk mewakili sebuah keadaan dalam permainan teka-teki delapan kotak (Eight Puzzle). Setiap instansi Node memiliki atribut state berupa array integer dengan panjang 9, yang menggambarkan susunan kotak-kotak pada papan teka-teki. Dalam kelas Node, terdapat atribut cost yang berperan dalam perhitungan biaya atau nilai dari fungsi heuristik. Node juga memiliki atribut parent yang berupa referensi ke Node yang menjadi induk dari Node saat ini dalam proses pencarian, berguna untuk melacak jalur solusi. Terdapat atribut successors yang berupa vektor yang berisi semua keadaan yang dapat dicapai dari Node saat ini. Kelas ini menyediakan berbagai metode seperti toString, equals, getPath, dan lainnya yang digunakan untuk operasi dan pemrosesan data dalam konteks algoritma A*.
* EightPuzzleSpace:
EightPuzzleSpace adalah sebuah kelas yang bertanggung jawab mengelola ruang pencarian dalam konteks permainan teka-teki delapan kotak. Fungsi utama dari kelas ini adalah untuk menyediakan metode yang memungkinkan pembuatan keadaan awal (root) dan tujuan dalam permainan. Metode getRoot digunakan untuk memperoleh keadaan awal dari permainan teka-teki delapan kotak, sedangkan metode getGoal digunakan untuk mendapatkan keadaan tujuan yang diharapkan dalam permainan. Kelas ini juga menyediakan metode getSuccessors yang bertugas menghasilkan semua kemungkinan keadaan yang dapat dicapai dari sebuah keadaan tertentu dalam permainan tersebut.
* EightPuzzleSearch:
EightPuzzleSearch adalah suatu kelas yang menerapkan algoritma pencarian A* guna menemukan solusi dalam permainan teka-teki delapan kotak. Kelas ini mempunyai atribut "space," yang merupakan instansi dari kelas EightPuzzleSpace untuk mengakses ruang pencarian. Terdapat juga atribut "open" dan "closed" yang merupakan vektor untuk memantau keadaan yang tengah dalam proses dan keadaan yang sudah diproses dalam pencarian. Metode h1Cost dan h2Cost dipakai untuk menghitung biaya heuristik berdasarkan dua metode heuristik yang berbeda, yaitu h1 dan h2. HCost adalah metode yang memungkinkan pemilihan metode heuristik yang akan digunakan selama pencarian, apakah h1 atau h2. Kemudian, metode getBestNode digunakan untuk mendapatkan Node dengan biaya terendah dari vektor nodes, sementara metode getPreviousCost berguna untuk mendapatkan biaya sebelumnya dari suatu Node.

2. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya  Gambar 8. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai goal  state. Analisa dan bedakan dengan solusi pada point 1.

Codingnya:
package JavaApplivation2;
import java.util.Vector;

class Node {
int[] state = new int[9];
int cost;
Node parent = null;
Vector<Node> successors = new Vector<Node>();

Node(int s[], Node parent) {
    this.parent = parent;
    for (int i = 0; i < 9; i++)
        state[i] = s[i];
}

public Node() {
}

public String toString() {
    String s = "";
    for (int i = 0; i < 9; i++) {
        s = s + state[i] + " ";
    }
    return s;
}

public boolean equals(Object node) {
    Node n = (Node) node;
    boolean result = true;
    for (int i = 0; i < 9; i++) {
        if (n.state[i] != state[i])
            result = false;
    }
    return result;
}

Vector<Node> getPath(Vector<Node> v) {
    v.insertElementAt(this, 0);
    if (parent != null)
        v = parent.getPath(v);
    return v;
}

Vector<Node> getPath() {
    return getPath(new Vector<Node>());
}

public static void main(String args[]) {
    new Node().run();
}

private void run() {
}
}

class EightPuzzleSpace {
Node getRoot() {
    int initialState[] = {3, 1, 2, 4, 7, 5, 6, 8, 0};
    return new Node(initialState, null);
}

Node getGoal() {
    int goalState[] = {1, 2, 3, 4, 0, 8, 5, 6, 7};
    return new Node(goalState, null);
}

Vector<Node> getSuccessors(Node parent) {
    Vector<Node> successors = new Vector<Node>();
    for (int r = 0; r < 3; r++) {
        for (int c = 0; c < 3; c++) {
            if (parent.state[(r * 3) + c] == 0) {
                if (r > 0) {
                    successors.add(transformState(r - 1, c, r, c, parent));
                }
                if (r < 2) {
                    successors.add(transformState(r + 1, c, r, c, parent));
                }
                if (c > 0) {
                    successors.add(transformState(r, c - 1, r, c, parent));
                }
                if (c < 2) {
                    successors.add(transformState(r, c + 1, r, c, parent));
                }
            }
        }
    }
    parent.successors = successors;
    return successors;
}

Node transformState(int r0, int c0, int r1, int c1, Node parent) {
    int[] s = parent.state;
    int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
    newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
    newState[(r0 * 3) + c0] = 0;
    return new Node(newState, parent);
}

public static void main(String args[]) {
    new EightPuzzleSpace().run();
}

private void run() {
}
}

class EightPuzzleSearch {
EightPuzzleSpace space = new EightPuzzleSpace();
Vector<Node> open = new Vector<Node>();
Vector<Node> closed = new Vector<Node>();

int h1Cost(Node node) {
    int cost = 0;
    for (int i = 0; i < node.state.length; i++) {
        if (node.state[i] != i) cost++;
    }
    return cost;
}

int h2Cost(Node node) {
    int cost = 0;
    int state[] = node.state;
    for (int i = 0; i < state.length; i++) {
        int v0 = i, v1 = state[i];
        if (v1 == 0)
            continue;
        int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
        int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
        cost += c;
    }
    return cost;
}

/* Boleh diubah dengan memakai heuristic h1 atau h2 */
int hCost(Node node) {
    return h1Cost(node);
}

Node getBestNode(Vector nodes) {
    int index = 0, minCost = Integer.MAX_VALUE;
    for (int i = 0; i < nodes.size(); i++) {
        Node node = (Node) nodes.elementAt(i);
        if (node.cost < minCost) {
            minCost = node.cost;
            index = i;
        }
    }
    Node bestNode = (Node) nodes.remove(index);
    return bestNode;
}

int getPreviousCost(Node node) {
    int i = open.indexOf(node);
    int cost = Integer.MAX_VALUE;
    if (i != -1) {
        cost = open.get(i).cost;
    } else {
        i = closed.indexOf(node);
        if (i != -1)
            cost = closed.get(i).cost;
    }
    return cost;
}

void printPath(Vector path) {
    for (int i = 0; i < path.size(); i++) {
        System.out.print(" " + path.elementAt(i) + "\n");
    }
}

void run() {
    Node root = space.getRoot();
    Node goal = space.getGoal();
    Node solution = null;
    open.add(root);
    System.out.print("\nRoot: " + root + "\n\n");
    while (open.size() > 0) {
        Node node = getBestNode(open);
        int pathLength = node.getPath().size();
        closed.add(node);
        if (node.equals(goal)) {
            solution = node;
            break;
        }
        Vector<Node> successors = space.getSuccessors(node);
        for (int i = 0; i < successors.size(); i++) {
            Node successor = successors.get(i);
            int cost = hCost(successor) + pathLength + 1;
            int previousCost;
            previousCost = getPreviousCost(successor);
            boolean inClosed;
            inClosed = closed.contains(successor);
            boolean inOpen = open.contains(successor);
            if (!(inClosed || inOpen) || cost < previousCost) {
                if (inClosed)
                    closed.remove(successor);
                if (!inOpen)
                    open.add(successor);
                successor.cost = cost;
                successor.parent = node;
            }
        }
    }
    if (solution != null) {
        Vector path = solution.getPath();
        System.out.print("\nSolution found\n");
        printPath(path);
    }
}

public static void main(String args[]) {
    new EightPuzzleSearch().run();
}
}

 output:
 ![image](https://github.com/hamdankf/Blind-search/assets/149086558/20a12acc-7677-4b18-8723-4895bbc6ff0c)

Dalam kode Eight Puzzle, proses pencarian solusi dari initial state ke goal state dijalankan menggunakan algoritma A* dengan pilihan heuristic h1 atau h2. Langkah-langkah pencarian melibatkan inisialisasi state awal dan tujuan, pembuatan node awal, dan pengelolaan vektor open dan closed. Selama vektor open masih berisi node, pencarian berlanjut dengan memilih node berbiaya terendah, menambahkannya ke vektor closed, dan mengevaluasi node penerus. Jika goal state tercapai, pencarian berakhir, dan solusi dicetak. Class EightPuzzleSearch, EightPuzzleSpace, dan Node memiliki peran yang berbeda, dengan EightPuzzleSearch mengatur algoritma, EightPuzzleSpace menyediakan informasi awal dan tujuan, dan Node merepresentasikan keadaan dalam permainan. Ketiganya bekerja sama untuk mengeksekusi solusi Eight Puzzle.

3. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya Gambar 5.9. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai  goal state. Analisa dan bedakan dengan solusi pada point 1 dan 2.
Coding :
package JavaApplivation2;
import java.util.Vector;

class Node {
int[] state = new int[9];
int cost;
Node parent = null;
Vector<Node> successors = new Vector<Node>();

Node(int s[], Node parent) {
    this.parent = parent;
    for (int i = 0; i < 9; i++)
        state[i] = s[i];
}

public Node() {
}

public String toString() {
    String s = "";
    for (int i = 0; i < 9; i++) {
        s = s + state[i] + " ";
    }
    return s;
}

public boolean equals(Object node) {
    Node n = (Node) node;
    boolean result = true;
    for (int i = 0; i < 9; i++) {
        if (n.state[i] != state[i])
            result = false;
    }
    return result;
}

Vector<Node> getPath(Vector<Node> v) {
    v.insertElementAt(this, 0);
    if (parent != null)
        v = parent.getPath(v);
    return v;
}

Vector<Node> getPath() {
    return getPath(new Vector<Node>());
}
}

class EightPuzzleSpace {
Node getRoot() {
    int initialState[] ={1, 5, 3, 4, 6, 8, 2, 7, 0};
    return new Node(initialState, null);
}

Node getGoal() {
    int goalState[] = {7, 6, 5, 8, 0, 4, 1, 2, 3};
    return new Node(goalState, null);
}

Vector<Node> getSuccessors(Node parent) {
    Vector<Node> successors = new Vector<Node>();
    for (int r = 0; r < 3; r++) {
        for (int c = 0; c < 3; c++) {
            if (parent.state[(r * 3) + c] == 0) {
                if (r > 0) {
                    successors.add(transformState(r - 1, c, r, c, parent));
                }
                if (r < 2) {
                    successors.add(transformState(r + 1, c, r, c, parent));
                }
                if (c > 0) {
                    successors.add(transformState(r, c - 1, r, c, parent));
                }
                if (c < 2) {
                    successors.add(transformState(r, c + 1, r, c, parent));
                }
            }
        }
    }
    parent.successors = successors;
    return successors;
}

Node transformState(int r0, int c0, int r1, int c1, Node parent) {
    int[] s = parent.state;
    int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
    newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
    newState[(r0 * 3) + c0] = 0;
    return new Node(newState, parent);
}

public static void main(String args[]) {
    new EightPuzzleSpace().run();
}

private void run() {
}
}

class EightPuzzleSearch {
EightPuzzleSpace space = new EightPuzzleSpace();
Vector<Node> open = new Vector<Node>();
Vector<Node> closed = new Vector<Node>();

int h1Cost(Node node) {
    int cost = 0;
    for (int i = 0; i < node.state.length; i++) {
    if (node.state[i] != i) cost++; }
    return cost;
    }

int h2Cost(Node node) {
    int cost = 0;
    int state[] = node.state;
    for (int i = 0; i < state.length; i++) {
        int v0 = i, v1 = state[i];
        if (v1 == 0)
            continue;
        int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
        int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
        cost += c;
    }
    return cost;
}
/*boleh diubah dengan memakai heuristic h1 atau h2 */
int hCost(Node node) {
return h1Cost(node);
}
Node getBestNode(Vector nodes) {
    int index = 0, minCost = Integer.MAX_VALUE;
    for (int i = 0; i < nodes.size(); i++) {
        Node node = (Node)nodes.elementAt(i);
        if (node.cost < minCost) {
            minCost = node.cost;
            index = i;
        }
    }
    Node bestNode = (Node)nodes.remove(index);
    return bestNode;
}

int getPreviousCost(Node node) {
    int i = open.indexOf(node);
    int cost = Integer.MAX_VALUE;
    if (i != -1) {
        cost = open.get(i).cost;
    } else {
        i = closed.indexOf(node);
        if (i != -1)
            cost = closed.get(i).cost;
    }
    return cost;
}

void printPath(Vector path) {
    for (int i = 0; i < path.size(); i++) {
        System.out.print(" " + path.elementAt(i) + "\n");
    }
}

void run() {
    Node root = space.getRoot();
    Node goal = space.getGoal();
    Node solution = null;
    open.add(root);
    System.out.print("\nRoot: " + root + "\n\n");
    while (open.size() > 0) {
        Node node = getBestNode(open);
        int pathLength = node.getPath().size();
        closed.add(node);
        if (node.equals(goal)) {
            solution = node;
            break;
        }
        Vector<Node> successors = space.getSuccessors(node);
        for (int i = 0; i < successors.size(); i++) {
            Node successor = successors.get(i);
            int cost = hCost(successor) + pathLength + 1;
            int previousCost;
            previousCost = getPreviousCost(successor);
            boolean inClosed;
            inClosed = closed.contains(successor);
            boolean inOpen = open.contains(successor);
            if (!(inClosed || inOpen) || cost < previousCost) {
                if (inClosed)
                    closed.remove(successor);
                if (!inOpen)
                    open.add(successor);
                successor.cost = cost;
                successor.parent = node;
            }
        }
    }
    if (solution != null) {
        Vector path = solution.getPath();
        System.out.print("\nSolution found\n");
        printPath(path);
    }
}

public static void main(String args[]) {
    new EightPuzzleSearch().run();
}
}

output:
![image](https://github.com/hamdankf/Blind-search/assets/149086558/31f46423-b44f-4a9f-886f-06b45ff8af27)

Implementasi kode di atas menggunakan algoritma A* untuk menyelesaikan permasalahan 8-puzzle. Algoritma ini dapat menggunakan dua heuristik, yaitu h1Cost dan h2Cost, yang memengaruhi jalannya algoritma. Langkah-langkah yang diperlukan untuk mencapai goal state dalam kode tersebut adalah sebagai berikut:
Kondisi awal, yang disebut sebagai "Root":
{1, 5, 3, 4, 6, 8, 2, 7, 0}
Kondisi goal yang ingin dicapai:
{7, 6, 5, 8, 0, 4, 1, 2, 3}
Algoritma A* dimulai dengan kondisi awal (Root) dan mengevaluasi semua langkah yang mungkin dari kondisi awal keadaan, dengan mempertimbangkan posisi kotak kosong. Algoritma ini menggunakan salah satu dari dua heuristik yang tersedia, yaitu h1Cost atau h2Cost, untuk menghitung biaya.
Selanjutnya, algoritma mencari jalur paling efisien ke goal state dengan memperhitungkan biaya total, yang terdiri dari biaya heuristik, jarak yang sudah ditempuh, dan biaya sebelumnya. Setiap langkah yang diambil untuk mencapai goal state dipilih berdasarkan prioritas biaya terendah, yang ditentukan oleh heuristik yang digunakan.
Algoritma A* akan terus berjalan hingga mencapai goal state. Perbedaan antara penggunaan h1Cost dan h2Cost dapat memengaruhi perhitungan biaya dan jalannya algoritma. H1Cost mengukur biaya dengan cara menghitung jumlah angka yang tidak berada di posisi yang benar, sementara h2Cost mengukur biaya berdasarkan jarak Manhatten antara angka yang salah dengan posisi yang seharusnya. Kedua heuristik ini bisa menghasilkan solusi yang berbeda dalam beberapa situasi.

4. Ubahlah initial dan goal state dari program di atas sehingga bentuk initial dan goal statenya  Gambar 5.10. Kemudian tentukan langkah-langkah mana saja sehingga puzzlenya mencapai  goal state. Analisa dan bedakan dengan solusi pada point 1, 2, dan 3.
Codingnya:
package JavaApplivation2;
import java.util.Vector;

class Node {
  int[] state = new int[9];
  int cost;
  Node parent = null;
  Vector<Node> successors = new Vector<Node>();

  Node(int s[], Node parent) {
      this.parent = parent;
      for (int i = 0; i < 9; i++)
          state[i] = s[i];
  }

  public Node() {
  }

  public String toString() {
      String s = "";
      for (int i = 0; i < 9; i++) {
          s = s + state[i] + " ";
      }
      return s;
  }

  public boolean equals(Object node) {
      Node n = (Node) node;
      boolean result = true;
      for (int i = 0; i < 9; i++) {
          if (n.state[i] != state[i])
              result = false;
      }
      return result;
  }

  Vector<Node> getPath(Vector<Node> v) {
      v.insertElementAt(this, 0);
      if (parent != null)
          v = parent.getPath(v);
      return v;
  }

  Vector<Node> getPath() {
      return getPath(new Vector<Node>());
  }
  public static void main(String args[]) {
      new Node().run();
  }

  private void run() {
  }
}

class EightPuzzleSpace {
  Node getRoot() {
      int initialState[] ={1, 2, 3, 4, 5, 6, 7, 8, 0};
      return new Node(initialState, null);
  }

  Node getGoal() {
      int goalState[] = {1, 2, 3, 4, 0, 5, 6, 7, 8};
      return new Node(goalState, null);
  }

  Vector<Node> getSuccessors(Node parent) {
      Vector<Node> successors = new Vector<Node>();
      for (int r = 0; r < 3; r++) {
          for (int c = 0; c < 3; c++) {
              if (parent.state[(r * 3) + c] == 0) {
                  if (r > 0) {
                      successors.add(transformState(r - 1, c, r, c, parent));
                  }
                  if (r < 2) {
                      successors.add(transformState(r + 1, c, r, c, parent));
                  }
                  if (c > 0) {
                      successors.add(transformState(r, c - 1, r, c, parent));
                  }
                  if (c < 2) {
                      successors.add(transformState(r, c + 1, r, c, parent));
                  }
              }
          }
      }
      parent.successors = successors;
      return successors;
  }

  Node transformState(int r0, int c0, int r1, int c1, Node parent) {
      int[] s = parent.state;
      int[] newState = { s[0], s[1], s[2], s[3], s[4], s[5], s[6], s[7], s[8] };
      newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
      newState[(r0 * 3) + c0] = 0;
      return new Node(newState, parent);
  }
  
  public static void main(String args[]) {
      new EightPuzzleSpace().run();
  }

  private void run() {
  }


  
  
}

class EightPuzzleSearch {
  EightPuzzleSpace space = new EightPuzzleSpace();
  Vector<Node> open = new Vector<Node>();
  Vector<Node> closed = new Vector<Node>();

  int h1Cost(Node node) {
      int cost = 0;
      for (int i = 0; i < node.state.length; i++) {
      if (node.state[i] != i) cost++; }
      return cost;
      }

  int h2Cost(Node node) {
      int cost = 0;
      int state[] = node.state;
      for (int i = 0; i < state.length; i++) {
          int v0 = i, v1 = state[i];
          if (v1 == 0)
              continue;
          int row0 = v0 / 3, col0 = v0 % 3, row1 = v1 / 3, col1 = v1 % 3;
          int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
          cost += c;
      }
      return cost;
  }
/*boleh diubah dengan memakai heuristic h1 atau h2 */
  int hCost(Node node) {
  return h1Cost(node);
  }
  Node getBestNode(Vector nodes) {
      int index = 0, minCost = Integer.MAX_VALUE;
      for (int i = 0; i < nodes.size(); i++) {
          Node node = (Node)nodes.elementAt(i);
          if (node.cost < minCost) {
              minCost = node.cost;
              index = i;
          }
      }
      Node bestNode = (Node)nodes.remove(index);
      return bestNode;
  }

  int getPreviousCost(Node node) {
      int i = open.indexOf(node);
      int cost = Integer.MAX_VALUE;
      if (i != -1) {
          cost = open.get(i).cost;
      } else {
          i = closed.indexOf(node);
          if (i != -1)
              cost = closed.get(i).cost;
      }
      return cost;
  }

  void printPath(Vector path) {
      for (int i = 0; i < path.size(); i++) {
          System.out.print(" " + path.elementAt(i) + "\n");
      }
  }

  void run() {
      Node root = space.getRoot();
      Node goal = space.getGoal();
      Node solution = null;
      open.add(root);
      System.out.print("\nRoot: " + root + "\n\n");
      while (open.size() > 0) {
          Node node = getBestNode(open);
          int pathLength = node.getPath().size();
          closed.add(node);
          if (node.equals(goal)) {
              solution = node;
              break;
          }
          Vector<Node> successors = space.getSuccessors(node);
          for (int i = 0; i < successors.size(); i++) {
              Node successor = successors.get(i);
              int cost = hCost(successor) + pathLength + 1;
              int previousCost;
              previousCost = getPreviousCost(successor);
              boolean inClosed;
              inClosed = closed.contains(successor);
              boolean inOpen = open.contains(successor);
              if (!(inClosed || inOpen) || cost < previousCost) {
                  if (inClosed)
                      closed.remove(successor);
                  if (!inOpen)
                      open.add(successor);
                  successor.cost = cost;
                  successor.parent = node;
              }
          }
      }
      if (solution != null) {
          Vector path = solution.getPath();
          System.out.print("\nSolution found\n");
          printPath(path);
      }
  }

  public static void main(String args[]) {
      new EightPuzzleSearch().run();
  }
}

 output:
 ![image](https://github.com/hamdankf/Blind-search/assets/149086558/31f46423-b44f-4a9f-886f-06b45ff8af27)

Implementasi "code 3" merupakan penerapan algoritma pencarian A* untuk menyelesaikan masalah 8-puzzle. Dalam implementasi ini, terdapat dua opsi heuristik yang dapat digunakan, yaitu h1Cost dan h2Cost, yang memengaruhi evaluasi biaya setiap langkah dalam pencarian solusi. 
Langkah-langkah yang diambil untuk menemukan solusi 8-puzzle dengan "code 3" adalah sebagai berikut:
Kondisi Awal (Root):
{1, 2, 3, 4, 5, 6, 7, 8, 0}
Kondisi Goal:
{1, 2, 3, 4, 0, 5, 6, 7, 8}
Proses algoritma A* dimulai dengan menginisialisasi kondisi awal (Root) dan mencari langkah-langkah yang diperlukan untuk mencapai goal state. Heuristik h1Cost atau h2Cost digunakan untuk menghitung biaya setiap langkah yang akan diambil. Algoritma memilih langkah dengan biaya terendah berdasarkan heuristik yang dipilih.
Algoritma akan terus berjalan hingga mencapai goal state, dan solusi yang spesifik untuk mencapai goal state {1, 2, 3, 4, 0, 5, 6, 7, 8} akan ditemukan dalam keluaran "code 3" setelah dijalankan.
Perbandingan dengan solusi pada poin 1, code 1, dan code 2 mengindikasikan bahwa solusi dalam "code 3" juga menggunakan algoritma A* untuk menyelesaikan masalah 8-puzzle, tetapi pilihan heuristik yang digunakan dapat memengaruhi urutan langkah-langkah dan waktu yang dibutuhkan untuk mencapai goal state. Dengan kata lain, hasil solusi dalam "code 3" bergantung pada heuristik yang digunakan (h1Cost atau h2Cost).

5. Ubahlah initial dan goal state dari program dan class-class di atas sehingga bentuk initial dan  goal statenya Gambar 5.11. Kemudian tentukan langkah-langkah mana saja sehingga  puzzlenya mencapai goal state.
Codingnya:
package JavaApplication2;
import java.util.Vector;

class Node {
char[] state = new char[9];
int cost;
Node parent = null;
Vector<Node> successors = new Vector<Node>();

Node(char s[], Node parent) {
    this.parent = parent;
    for (int i = 0; i < 9; i++)
        state[i] = s[i];
}

public Node() {
}

public String toString() {
    String s = "";
    for (int i = 0; i < 9; i++) {
        s = s + state[i] + " ";
    }
    return s;
}

public boolean equals(Object node) {
    Node n = (Node) node;
    boolean result = true;
    for (int i = 0; i < 9; i++) {
        if (n.state[i] != state[i])
            result = false;
    }
    return result;
}

Vector<Node> getPath(Vector<Node> v) {
    v.insertElementAt(this, 0);
    if (parent != null)
        v = parent.getPath(v);
    return v;
}

Vector<Node> getPath() {
    return getPath(new Vector<Node>());
}

public static void main(String args[]) {
    new Node().run();
}

private void run() {
}
}

class EightPuzzleSpace {
Node getRoot() {
    char initialState[] = {'D', 'B', 'E', 'A', 'F', 'G', 'H', 'C', 'x'};
    return new Node(initialState, null);
}

Node getGoal() {
    char goalState[] = {'A', 'H', 'G', 'B', 'x', 'F', 'C', 'D', 'E'};
    return new Node(goalState, null);
}

Vector<Node> getSuccessors(Node parent) {
    Vector<Node> successors = new Vector<Node>();
    for (int r = 0; r < 3; r++) {
        for (int c = 0; c < 3; c++) {
            if (parent.state[(r * 3) + c] == 'x') {
                if (r > 0) {
                    successors.add(transformState(r - 1, c, r, c, parent));
                }
                if (r < 2) {
                    successors.add(transformState(r + 1, c, r, c, parent));
                }
                if (c > 0) {
                    successors.add(transformState(r, c - 1, r, c, parent));
                }
                if (c < 2) {
                    successors.add(transformState(r, c + 1, r, c, parent));
                }
            }
        }
    }
    parent.successors = successors;
    return successors;
}

Node transformState(int r0, int c0, int r1, int c1, Node parent) {
    char[] s = parent.state;
    char[] newState = {'x', 'x', 'x', 'x', 'x', 'x', 'x', 'x', 'x'};
    newState[(r1 * 3) + c1] = s[(r0 * 3) + c0];
    newState[(r0 * 3) + c0] = 'x';
    return new Node(newState, parent);
}

public static void main(String args[]) {
    new EightPuzzleSpace().run();
}

private void run() {
}
}

class EightPuzzleSearch {
EightPuzzleSpace space = new EightPuzzleSpace();
Vector<Node> open = new Vector<Node>();
Vector<Node> closed = new Vector<Node>();

int h1Cost(Node node) {
    int cost = 0;
    for (int i = 0; i < node.state.length; i++) {
        if (node.state[i] != 'x')
            cost++;
    }
    return cost;
}

int h2Cost(Node node) {
    int cost = 0;
    char state[] = node.state;
    for (int i = 0; i < state.length; i++) {
        char v = state[i];
        if (v != 'x') {
            int row0 = i / 3, col0 = i % 3;
            int row1 = i / 3, col1 = i % 3;
            if (v != 'x') {
                int c = (Math.abs(row0 - row1) + Math.abs(col0 - col1));
                cost += c;
            }
        }
    }
    return cost;
}

int hCost(Node node) {
    return h1Cost(node);
}

Node getBestNode(Vector nodes) {
    int index = 0, minCost = Integer.MAX_VALUE;
    for (int i = 0; i < nodes.size(); i++) {
        Node node = (Node) nodes.elementAt(i);
        if (node.cost < minCost) {
            minCost = node.cost;
            index = i;
        }
    }
    Node bestNode = (Node) nodes.remove(index);
    return bestNode;
}

int getPreviousCost(Node node) {
    int i = open.indexOf(node);
    int cost = Integer.MAX_VALUE;
    if (i != -1) {
        cost = open.get(i).cost;
    } else {
        i = closed.indexOf(node);
        if (i != -1)
            cost = closed.get(i).cost;
    }
    return cost;
}

void printPath(Vector path) {
    for (int i = 0; i < path.size(); i++) {
        System.out.print(" " + path.elementAt(i) + "\n");
    }
}

void run() {
    Node root = space.getRoot();
    Node goal = space.getGoal();
    Node solution = null;
    open.add(root);
    System.out.print("\nRoot: " + root + "\n\n");
    while (open.size() > 0) {
        Node node = getBestNode(open);
        int pathLength = node.getPath().size();
        closed.add(node);
        if (node.equals(goal)) {
            solution = node;
            break;
        }
        Vector<Node> successors = space.getSuccessors(node);
        for (int i = 0; i < successors.size(); i++) {
            Node successor = successors.get(i);
            int cost = hCost(successor) + pathLength + 1;
            int previousCost;
            previousCost = getPreviousCost(successor);
            boolean inClosed;
            inClosed = closed.contains(successor);
            boolean inOpen = open.contains(successor);
            if (!(inClosed || inOpen) || cost < previousCost) {
                if (inClosed)
                    closed.remove(successor);
                if (!inOpen)
                    open.add(successor);
                successor.cost = cost;
                successor.parent = node;
            }
        }
    }
    if (solution != null) {
        Vector path = solution.getPath();
        System.out.print("\nSolution found\n");
        printPath(path);
    }
}

public static void main(String args[]) {
    new EightPuzzleSearch().run();
}
}

 output:
 ![image](https://github.com/hamdankf/Blind-search/assets/149086558/bbb5792c-1e01-4b57-9466-8085c0cc258e)

Untuk menemukan solusi dalam Code 4 guna mencapai goal state dalam permainan puzzle, langkah-langkah dapat didefinisikan dengan menggunakan algoritma pencarian seperti A* Search yang mengggunakan dua heuristik, yaitu h1Cost dan h2Cost yang tersedia dalam kode. Langkah-langkah umumnya adalah sebagai berikut:

1. Inisialisasi node awal dengan kondisi awal dari puzzle.

2. Selanjutnya, inisialisasi open list dan closed list sebagai wadah untuk melacak status node-node. Node awal ditempatkan ke dalam open list.

3. Selama open list masih memiliki node-node yang belum dieksplorasi, jalankan langkah-langkah berikut:
   a. Ambil node dengan biaya terendah dari open list, di mana biaya tersebut dihitung sebagai kombinasi biaya heuristik dan panjang path.
   b. Jika node yang diambil adalah goal state, maka pencarian selesai, dan solusi telah ditemukan.
   c. Selanjutnya, hasilkan semua node turunan dari node saat ini.
   d. Untuk setiap node turunan, hitung biaya f (menggunakan salah satu dari h1Cost atau h2Cost) dan panjang path dari node awal hingga node tersebut.
   e. Jika node turunan sudah ada dalam open list dan biaya f yang baru lebih rendah, maka perbarui biaya dan parent node tersebut.
   f. Jika node turunan sudah ada dalam closed list dan biaya f yang baru lebih rendah, maka perbarui biaya dan parent node tersebut, dan pindahkan node dari closed list ke open list.
   g. Jika node turunan belum ada dalam open list atau closed list, tambahkan node tersebut ke dalam open list.
   h. Akhiri langkah dengan menandai node saat ini sebagai telah dieksplorasi, dengan cara memindahkannya ke dalam closed list.

Setelah pencarian selesai dan goal state berhasil ditemukan, langkah-langkah untuk mencapai goal state dapat ditentukan dengan mengikuti parent nodes dari goal state. Dengan demikian, solusi dapat ditemukan dengan mengikuti urutan langkah-langkah yang diperlukan untuk mencapai goal state.

