#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Structure to represent an edge
struct Edge {
    int u, v, weight;
};

// Function to find the parent of a node
int findParent(int node, int parent[]) {
    if (parent[node] == node) 
        return node;
    return parent[node] = findParent(parent[node], parent); // Path compression
}

// Function to perform union of two nodes
void unionNodes(int u, int v, int parent[], int rank[]) {
    u = findParent(u, parent);
    v = findParent(v, parent);
    if (u != v) {
        if (rank[u] > rank[v]) 
            parent[v] = u;
        else if (rank[u] < rank[v]) 
            parent[u] = v;
        else {
            parent[v] = u;
            rank[u]++;
        }
    }
}

// Comparator function to sort edges by weight
bool compare(Edge a, Edge b) {
    return a.weight < b.weight;
}

// Function to find MST using Kruskal's Algorithm
void kruskalMST(int n, vector<Edge>& edges) {
    // Sort edges by weight
    sort(edges.begin(), edges.end(), compare);

    // Initialize parent and rank arrays
    int parent[n], rank[n];
    for (int i = 0; i < n; i++) {
        parent[i] = i;
        rank[i] = 0;
    }

    vector<pair<int, int>> mstEdges; // Edges in MST
    int mstCost = 0;

    // Process each edge
    for (auto edge : edges) {
        if (findParent(edge.u, parent) != findParent(edge.v, parent)) {
            unionNodes(edge.u, edge.v, parent, rank);
            mstEdges.push_back({edge.u, edge.v});
            mstCost += edge.weight;
        }
    }

    // Output the result
    cout << "Edges in MST: ";
    for (auto edge : mstEdges) {
        cout << "(" << edge.first << ", " << edge.second << ") ";
    }
    cout << endl;
    cout << "Total Cost: " << mstCost << endl;
}

int main() {
    // Input number of cities (nodes) and edges
    int n, m;
    cout << "Enter number of cities (nodes) and edges: ";
    cin >> n >> m;

    vector<Edge> edges(m);
    cout << "Enter edges (city1 city2 cost):" << endl;
    for (int i = 0; i < m; i++) {
        cin >> edges[i].u >> edges[i].v >> edges[i].weight;
    }

    // Call the Kruskal's MST function
    kruskalMST(n, edges);

    return 0;
}
