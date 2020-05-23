from collections import defaultdict

INF = float('INF')

def minDistance(dist, visited):
    (minimum, minVertex) = (INF, 0)
    for vertex in range(len(dist)):
        if minimum > dist[vertex] and visited[vertex] == False:
            (minimum, minVertex) = (dist[vertex], vertex)

    return minVertex


def Dijkstra(graph, modifiedGraph, src):
    u = []
    num_vertices = len(graph)
    sptSet = defaultdict(lambda: False)
    dist = [INF] * num_vertices
    dist[src] = 0

    for count in range(num_vertices):
        curVertex = minDistance(dist, sptSet)
        sptSet[curVertex] = True
        for vertex in range(num_vertices):
            if ((sptSet[vertex] == False) and (dist[vertex] > (dist[curVertex] + modifiedGraph[curVertex][vertex])) and (graph[curVertex][vertex] != 0)):
                dist[vertex] = (dist[curVertex] + modifiedGraph[curVertex][vertex]);
    for vertex in range(num_vertices):
        u.append(int(str(dist[vertex])))
    return u[0:(len(u))]
def BellmanFord(edges, graph, num_vertices):
    dist = [INF] * (num_vertices + 1)
    dist[num_vertices] = 0

    for i in range(num_vertices):
        edges.append([num_vertices, i, 0])

    for i in range(num_vertices):
        for (src, des, weight) in edges:
            if ((dist[src] != INF) and (dist[src] + weight < dist[des])):
                dist[des] = dist[src] + weight
    return dist[0:num_vertices]


def JohnsonAlgorithm(graph):
    edges = []
    w = []

    for i in range(len(graph)):
        for j in range(len(graph[i])):
            if graph[i][j] != 0:
                edges.append([i, j, graph[i][j]])


    modifyWeights = BellmanFord(edges, graph, len(graph))
    modifiedGraph = [[0 for x in range(len(graph))] for y in range(len(graph))]


    for i in range(len(graph)):
        for j in range(len(graph[i])):

            if graph[i][j] != 0:
                modifiedGraph[i][j] = (graph[i][j] + modifyWeights[i] - modifyWeights[j]);
    for src in range(len(graph)):

        m =Dijkstra(graph, modifiedGraph, src)
        w.append(m)
    return w


graph = [[0, 3, 8, 0, -4],
        [0, 0, 0, -1, 7],
        [0, 4, 0, 0, 0],
        [2, 0, 5, 0, 0],
        [0, 0, 0, 6, 0]]

R = JohnsonAlgorithm(graph)
print("\n\n  A continuación se muestra la Matriz de distancias más cortas: \n\n", R)
