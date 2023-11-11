# 5311421117_Lanang-Setiawan_Tugas-Modul-Praktikum-4-Pencarian-Blind-Search  

## Nama  : Lanang Setiawan
## NIM   : 5311421117

# A. Tujuan Praktikum 
Mahasiswa mampu memahami konsep blind search dan dapat mengimplementasikan program salah satu algoritma blind search pada kasus tree. Program ini dibuat dengan menggunakan bahasa pemrograman Java.

# B. Dasar Teori
Blind search merupakan sebuah metode dalam ilmu komputer dan teori permainan yang digunakan untuk mengeksplorasi ruang pencarian tanpa pengetahuan sebelumnya tentang struktur atau sifatnya. Dalam konteks ini, ruang pencarian adalah himpunan semua kemungkinan keadaan atau solusi dalam sebuah masalah. Blind search dimulai dari keadaan awal dan bertujuan untuk mencapai keadaan tujuan. Operator atau tindakan digunakan untuk berpindah antara keadaan-keadaan dalam ruang pencarian, dan algoritma blind search sering menggunakan antrian untuk mengatur urutan eksplorasi. Dua contoh algoritma blind search yang umum adalah pencarian dalam lebar, yang menjelajahi keadaan dalam urutan berdasarkan kedalaman, dan pencarian dalam dalam, yang lebih mendalam sebelum kembali ke cabang-cabang belum dieksplorasi. Blind search memiliki kelebihan dalam kesederhanaan dan keuniversalan, tetapi kekurangannya adalah efisiensinya dalam ruang pencarian yang besar atau kompleks. Meskipun demikian, blind search adalah dasar penting dalam pengembangan algoritma pencarian yang lebih canggih dalam bidang kecerdasan buatan dan pemecahan masalah.

# C. Coding

    import java.util.ArrayDeque;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Queue;
    import java.util.Set;

    public class BlindSearch {
        public enum NodeColour {
            WHITE, GRAY, BLACK
    }

    public static class Node {
        int data;
        int distance;
        Node predecessor;
        NodeColour colour;

    public Node(int data)

    {
          this.data = data;
    }

        public String toString() {
            return "(" + data + ",d=" + distance + ")";
        }
    }

    Map<Node, List<Node>> nodes;

    public BlindSearch()
    {
    nodes = new HashMap<Node, List<Node>>();
    }

    public void addEdge(Node n1, Node n2) {
        if (nodes.containsKey(n1)) {
            nodes.get(n1).add(n2);
        } else {
            ArrayList<Node> list = new ArrayList<Node>();
            list.add(n2);
            nodes.put(n1, list);
        }
    }

    public void bfs(Node s) {
        Set<Node> keys = nodes.keySet();
        for (Node u : keys) {
            if (u != s) {
                u.colour = NodeColour.WHITE;
                u.distance = Integer.MAX_VALUE;
                u.predecessor = null;
            }
        }
        s.colour = NodeColour.GRAY;
        s.distance = 0;
        s.predecessor = null;
        Queue<Node> q = new ArrayDeque<Node>();
        q.add(s);
        while (!q.isEmpty()) {
            Node u = q.remove();
            List<Node> adj_u = nodes.get(u);
            if (adj_u != null) {
                for (Node v : adj_u) {
                    if (v.colour == NodeColour.WHITE) {
                        v.colour = NodeColour.GRAY;
                        v.distance = u.distance + 1;
                        v.predecessor = u;
                        q.add(v);
                    }
                }
            }
            u.colour = NodeColour.BLACK;
            System.out.print(u + " ");
        }
    }

    public static void main(String[] args)

    {
        BlindSearch graph = new BlindSearch();
        Node n1 = new Node(1);
        Node n2 = new Node(2);
        Node n3 = new Node(3);
        Node n4 = new Node(4);
        Node n5 = new Node(5);
        Node n6 = new Node(6);
        Node n7 = new Node(7);
        Node n8 = new Node(8);
        
        graph.addEdge(n1, n2);
        
        graph.addEdge(n2, n1);
        graph.addEdge(n2, n3);
        
        graph.addEdge(n3, n4);
        graph.addEdge(n3, n2);
        
        graph.addEdge(n4, n3);
        graph.addEdge(n4, n5);
        graph.addEdge(n4, n6);
        
        graph.addEdge(n5, n4);
        graph.addEdge(n5, n6);
        graph.addEdge(n5, n7);
        
        graph.addEdge(n6, n4);
        graph.addEdge(n6, n5);
        graph.addEdge(n6, n7);
        graph.addEdge(n6, n8);
        
        graph.addEdge(n7, n5);
        graph.addEdge(n7, n6);
        graph.addEdge(n7, n8);
        
        graph.addEdge(n8, n6);
        graph.addEdge(n8, n7);
        
        graph.bfs(n3);
      }
    }

Untuk kode diatas memiliki output (3,d=0) (4,d=1) (2,d=1) (5,d=2) (6,d=2) (1,d=2) (7,d=3) (8,d=3) 
 
![image](https://github.com/hamdankf/Blind-search/assets/149086558/7da1bb58-ab92-44c2-b609-06c1f6597657)

# D. Tugas
1. Tentukan bagaimana algoritma BFS di atas dapat menentukan node ke 8, 6, dan 7.
2. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.5 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 5.
3. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.6 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 9.
4. Ubahlah kode program di atas sehingga bentuk tree seperti Gambar 4.7 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node C.

# E. Jawaban
 1. Cara BFS di atas dapat menentukan node ke 8, 6, dan 7?

1. Dimulai dari node awal, yaitu node 3 (n3), yang digunakan sebagai titik awal yang diteruskan ke metode bfs(n3).
2. Pada awal algoritma, semua node dalam graf diinisialisasi dengan warna putih (WHITE), kecuali node awal (n3), yang diberi warna abu-abu (GRAY) karena sedang dalam proses.
3. Node awal (n3) memiliki jarak (distance) sebesar 0.
4. Node-node yang langsung terhubung dengan node awal (n3) adalah node 2 (n2) dan node 4 (n4). Kedua node ini dimasukkan ke dalam antrian (queue) untuk diproses selanjutnya.
5. Algoritma BFS akan melakukan iterasi selama antrian masih berisi node.
6. Node yang saat ini sedang diproses adalah node 3 (n3), yang memiliki daftar keterhubungan (adjacency list) berisi node 2 (n2) dan node 4 (n4).
7. Node 2 (n2) dan node 4 (n4) adalah node-node berwarna putih (WHITE) karena belum diproses. Algoritma BFS akan mengubah warna node 2 (n2) dan node 4 (n4) menjadi abu-abu (GRAY), menghitung jaraknya, dan menetapkan node 3 (n3) sebagai pendahulunya.
    - Node 2 (n2) akan memiliki jarak 1 dari node awal (n3).
    - Node 4 (n4) akan memiliki jarak 1 dari node awal (n3).
8. Node-node 2 (n2) dan 4 (n4) dimasukkan ke dalam antrian (queue) untuk diproses pada tahap berikutnya.
9. Algoritma akan terus berlanjut dengan mengambil node-node dari antrian berikutnya untuk diproses.
10. Algoritma akan terus melakukan langkah-langkah serupa hingga semua node yang terhubung dalam graph telah selesai diproses.
11. Node-node yang telah selesai diproses akan diberi warna hitam (BLACK).
12. Hasil dari algoritma BFS adalah pencarian jarak terpendek dari node awal (n3) ke semua node lain dalam graf, serta penentuan node pendahulu untuk setiap node. Oleh karena itu, informasi ini dapat digunakan untuk menentukan jarak dari node awal (n3) ke node 8 (n8), node 6 (n6), dan node 7 (n7).
= Jadi Node 8 (n8) terhubung dengan node 6 (n6), yang terhubung dengan node 4 (n4), yang terhubung dengan node 3 (n3). Jarak dari n3 ke n4 adalah 1, jarak dari n4 ke n6 adalah 1, dan jarak dari n6 ke n8 adalah 1. Jadi, jarak dari n3 ke n8 adalah 3. Node 7 (n7) juga terhubung dengan node 6 (n6), yang terhubung dengan node 4 (n4), yang terhubung dengan node 3 (n3). Jarak dari n3 ke n4 adalah 1, jarak dari n4 ke n6 adalah 1, dan jarak dari n6 ke n7 adalah 1. Jadi, jarak dari n3 ke n7 adalah 3.

 2.  Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.5 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 5?
Yang pertama kita ubah pada bagian static void nya menjadi coding seperti berikut :

import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Set;

public class NewMain {
public enum NodeColour { WHITE, GRAY, BLACK }

public static class Node {
    int data;
    int distance;
    Node predecessor;
    NodeColour colour;

    public Node(int data) {
        this.data = data;
    }

    public String toString() {
        return "(" + data + ", d=" + distance + ")";
    }
}

Map<Node, List<Node>> nodes;

public NewMain() {
    nodes = new HashMap<Node, List<Node>>();
}

public void addEdge(Node n1, Node n2) {
    if (nodes.containsKey(n1)) {
        nodes.get(n1).add(n2);
    } else {
        ArrayList<Node> list = new ArrayList<Node>();
        list.add(n2);
        nodes.put(n1, list);
    }
}

public void bfs(Node s) {
    Set<Node> keys = nodes.keySet();
    for (Node u : keys) {
        if (u != s) {
            u.colour = NodeColour.WHITE;
            u.distance = Integer.MAX_VALUE;
            u.predecessor = null;
        }
    }
    s.colour = NodeColour.GRAY;
    s.distance = 0;
    s.predecessor = null;
    Queue<Node> q = new ArrayDeque<Node>();
    q.add(s);
    while (!q.isEmpty()) {
        Node u = q.poll(); //q.poll()
        List<Node> adj_u = nodes.get(u);
        if (adj_u != null) {
            for (Node v : adj_u) {
                if (v.colour == NodeColour.WHITE) {
                    v.colour = NodeColour.GRAY;
                    v.distance = u.distance + 1;
                    v.predecessor = u;
                    q.add(v);
                }
            }
        }
        u.colour = NodeColour.BLACK;
        System.out.print(u + " ");
    }
}

public static void main(String[] args) {
    NewMain graph = new NewMain(); // "NewMain"
    Node n0 = new Node(0);
    Node n1 = new Node(1);
    Node n2 = new Node(2);
    Node n3 = new Node(3);
    Node n4 = new Node(4);
    Node n5 = new Node(5);
    Node n6 = new Node(6);

    graph.addEdge(n0, n1);
    graph.addEdge(n0, n2);

    graph.addEdge(n1, n0);
    graph.addEdge(n1, n3);
    graph.addEdge(n1, n4);

    graph.addEdge(n2, n0);
    graph.addEdge(n2, n5);
    graph.addEdge(n2, n6);

    graph.addEdge(n3, n1);
    graph.addEdge(n3, n4);

    graph.addEdge(n4, n3);
    graph.addEdge(n4, n1);

    graph.addEdge(n5, n2);
    graph.addEdge(n5, n6);
    graph.addEdge(n5, n1);

    graph.addEdge(n6, n2);
    graph.addEdge(n6, n5);
    graph.addEdge(n6, n1);

    graph.bfs(n0);
}
}
kode diatas memberikan output (0,d=0) (1,d=1) (2,d=1) (3,d=2) (4,d=2) (5,d=2) (6,d=2) 
![image](https://github.com/hamdankf/Blind-search/assets/149086558/88169284-ec0a-45de-9a0e-3f4603f8580c)
 
node 5 (n5) terhubung dengan node 2 (n2) yang terhubung dengan node 0 (n0). jarak n0 ke n2 adalah 1 dan jarak n2 ke n5 adalah 1 jadi jarak n0 ke n5 adalah 2.

3. Ubahlah method static void main sehingga bentuk tree seperti Gambar 4.6 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node 9?
Yang pertama kita ubah static void nya menjadikannya coding seperti berikut:

import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Set;

public class NewMain {
public enum NodeColour { WHITE, GRAY, BLACK }

public static class Node {
    int data;
    int distance;
    Node predecessor;
    NodeColour colour;

    public Node(int data) {
        this.data = data;
    }

    public String toString() {
        return "(" + data + ", d=" + distance + ")";
    }
}

Map<Node, List<Node>> nodes;

public NewMain() {
    nodes = new HashMap<Node, List<Node>>();
}

public void addEdge(Node n1, Node n2) {
    if (nodes.containsKey(n1)) {
        nodes.get(n1).add(n2);
    } else {
        ArrayList<Node> list = new ArrayList<Node>();
        list.add(n2);
        nodes.put(n1, list);
    }
}

public void bfs(Node s) {
    Set<Node> keys = nodes.keySet();
    for (Node u : keys) {
        if (u != s) {
            u.colour = NodeColour.WHITE;
            u.distance = Integer.MAX_VALUE;
            u.predecessor = null;
        }
    }
    s.colour = NodeColour.GRAY;
    s.distance = 0;
    s.predecessor = null;
    Queue<Node> q = new ArrayDeque<Node>();
    q.add(s);
    while (!q.isEmpty()) {
        Node u = q.poll();
        List<Node> adj_u = nodes.get(u);
        if (adj_u != null) {
            for (Node v : adj_u) {
                if (v.colour == NodeColour.WHITE) {
                    v.colour = NodeColour.GRAY;
                    v.distance = u.distance + 1;
                    v.predecessor = u;
                    q.add(v);
                }
            }
        }
        u.colour = NodeColour.BLACK;
        System.out.print(u + " ");
    }
}

public static void main(String[] args) {
    NewMain graph = new NewMain(); //NewMain
    Node n1 = new Node(1);
    Node n2 = new Node(2);
    Node n3 = new Node(3);
    Node n4 = new Node(4);
    Node n5 = new Node(5);
    Node n6 = new Node(6);
    Node n7 = new Node(7);
    Node n8 = new Node(8);
    Node n9 = new Node(9);
    Node n10 = new Node(10);
    Node n11 = new Node(11);
    Node n12 = new Node(12);

    // Tambahkan edge sesuai dengan yang diinginkan
    graph.addEdge(n1, n2);
    graph.addEdge(n1, n3);
    graph.addEdge(n1, n4);

    graph.addEdge(n2, n1);
    graph.addEdge(n2, n5);
    graph.addEdge(n2, n6);

    graph.addEdge(n3, n1);

    graph.addEdge(n4, n1);
    graph.addEdge(n4, n7);
    graph.addEdge(n4, n8);

    graph.addEdge(n5, n2);
    graph.addEdge(n5, n9);
    graph.addEdge(n5, n10);
    graph.addEdge(n5, n6);

    graph.addEdge(n6, n2);
    graph.addEdge(n6, n5);

    graph.addEdge(n7, n4);
    graph.addEdge(n7, n11);
    graph.addEdge(n7, n12);
    graph.addEdge(n7, n8);

    graph.addEdge(n8, n4);
    graph.addEdge(n8, n7);

    graph.addEdge(n9, n5);
    graph.addEdge(n9, n10);

    graph.addEdge(n10, n5);
    graph.addEdge(n10, n9);

    graph.addEdge(n11, n7);
    graph.addEdge(n11, n12);

    graph.addEdge(n12, n7);
    graph.addEdge(n12, n11);

    graph.bfs(n1);
}
}
kode diatas memberikan output (1,d=0) (2,d=1) (3,d=1) (4,d=1) (5,d=2) (6,d=2) (7,d=2) (8,d=2) (9,d=3) (10,d=3) (11,d=3) (12,d=3)

 ![image](https://github.com/hamdankf/Blind-search/assets/149086558/24c6b871-4d24-4cf9-a394-774ef3b957a7)

Node 9 (n9) memiliki keterhubungan dengan node 5 (n5), yang kemudian terhubung dengan node 2 (n2), yang selanjutnya terhubung dengan node 1 (n1). Jarak dari node 1 ke node 2 adalah 1, jarak dari node 2 ke node 5 adalah 1, dan jarak dari node 5 ke node 9 adalah 1. Oleh karena itu, total jarak dari node 1 ke node 9 adalah 4.
 4. Ubahlah kode program di atas sehingga bentuk tree seperti Gambar 4.7 dapat dibentuk. Kemudian tentukan bagaimana algoritma BFS dapat menemukan node C ?
Yang pertama ubah static void nya dan Dan juga pada bagian yang bertuliskan int data yaitu pada public class dan public Node diganti dengan char karena kita menggunakan huruf bukan menggunakan angka lalu menjadi coding seperti dibawah:

    import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Queue;
import java.util.Set;

public class NewMain {
public enum NodeColour { WHITE, GRAY, BLACK }

public static class Node {
    char data;
    int distance;
    Node predecessor;
    NodeColour colour;

    public Node(char data) {
        this.data = data;
    }

    public String toString() {
        return "(" + data + ", d=" + distance + ")";
    }
}

Map<Node, List<Node>> nodes;

public NewMain() {
    nodes = new HashMap<Node, List<Node>>();
}

public void addEdge(Node n1, Node n2) {
    if (nodes.containsKey(n1)) {
        nodes.get(n1).add(n2);
    } else {
        ArrayList<Node> list = new ArrayList<Node>();
        list.add(n2);
        nodes.put(n1, list);
    }
}

public void bfs(Node s) {
    Set<Node> keys = nodes.keySet();
    for (Node u : keys) {
        if (u != s) {
            u.colour = NodeColour.WHITE;
            u.distance = Integer.MAX_VALUE;
            u.predecessor = null;
        }
    }
    s.colour = NodeColour.GRAY;
    s.distance = 0;
    s.predecessor = null;
    Queue<Node> q = new ArrayDeque<Node>();
    q.add(s);
    while (!q.isEmpty()) {
        Node u = q.poll();
        List<Node> adj_u = nodes.get(u);
        if (adj_u != null) {
            for (Node v : adj_u) {
                if (v.colour == NodeColour.WHITE) {
                    v.colour = NodeColour.GRAY;
                    v.distance = u.distance + 1;
                    v.predecessor = u;
                    q.add(v);
                }
            }
        }
        u.colour = NodeColour.BLACK;
        System.out.print(u + " ");
    }
}

public static void main(String[] args) {
    NewMain graph = new NewMain();
    Node n1 = new Node('A');
    Node n2 = new Node('B');
    Node n3 = new Node('C');
    Node n4 = new Node('D');
    Node n5 = new Node('E');
    Node n6 = new Node('F');
    Node n7 = new Node('G');
    Node n8 = new Node('H');
    Node n9 = new Node('I');

    graph.addEdge(n1, n2);
    graph.addEdge(n1, n4);

    graph.addEdge(n2, n6);
    graph.addEdge(n2, n1);
    graph.addEdge(n2, n4);

    graph.addEdge(n3, n4);
    graph.addEdge(n3, n5);

    graph.addEdge(n4, n2);
    graph.addEdge(n4, n3);
    graph.addEdge(n4, n5);

    graph.addEdge(n5, n4);
    graph.addEdge(n5, n3);

    graph.addEdge(n6, n2);
    graph.addEdge(n6, n7);
    
    graph.addEdge(n7, n6);
    graph.addEdge(n7, n9);

    graph.addEdge(n8, n9);
    
    graph.addEdge(n9, n8);

    graph.bfs(n6);
}
}

kode diatas memberikan output (F,d=0) (B,d=1) (G,d=1) (A,d=2) (D,d=2) (I,d=2) (C,d=3) (E,d=3) (H,d=3)

![image](https://github.com/lanangsetiawan/5311421117_Lanang-Setiawan_Tugas-Modul-Praktikum-4-Pencarian-Blind-Search/blob/main/Blind%20Search.png)
