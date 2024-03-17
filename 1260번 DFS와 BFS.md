## 문제

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.

## 입력

첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.

## 출력

첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다.

## 풀이

```
let fs = require("fs");
let input = fs.readFileSync("index.txt").toString().trim().split('\n');

//dfs => 깊이
//bfs => 너비

//우선 정점 / 간선의 정보로 그래프를 만들고
//dfs bfs 탐색 함수를 만들어서 결과를 찍어낸다

//정점의 개수, 간선의 개수, 정점의 번호
let [n,m,v] = input[0].split(' ').map(Number);

//간선이 연결하는 두 정점의 번호
let edges = input.slice(1).map(v => v.split(' ').map(Number));

//1부터 시작하니깐 0은 버리고 시작
let graph = [];
for(let i=1; i<=n; i++) graph[i] = [];

//쌍으로 연결된 정점을 인접리스트로 만들어준
for(let [from, to] of edges){
  graph[from].push(to);
  graph[to].push(from);
}

//정점 번호가 작은 것을 먼저 방문하고 ... 그럼 오름차순?
graph.forEach(v => v.sort((a, b) => a - b));

//dfs 탐색
//방문여부 체크
let dfs_visited = new Array(n+1).fill(false);
let dfs_graph = [];

function dfs(v){
  //정점의 번호부터 들어온다
  dfs_visited[v] = true; //방문했습니다.
  dfs_graph.push(v); //출력을 위해 리스트에 노드 삽입

  //연결되어있는 노드를 탐색해준다.
  for(let node of graph[v]){
    //방문하지않았다면 탐색
    if(!dfs_visited[node]) dfs(node);
  }
}

//bfs 탐색
let bfs_visited = new Array(n+1).fill(false);
let bfs_graph = [];

function bfs(v){
  let queue = [];
  //큐에 넣고
  queue.push(v);
  //방문처리 해준다.
  bfs_visited[v] = true;

  //큐가 빌때까지 계속 반복
  while(queue.length !== 0){
    //원소를 꺼내서 방문하지 않은 인접노드가 있는지 확인한다.
    //큐이기 때문에 앞에서 빼줘야됨
    let dequeue = queue.shift();
    bfs_graph.push(dequeue); //출력을 위해 리스트에 노드 삽입

    for(let node of graph[dequeue]){
      //아직 방문하지 않은 인접노드를 큐에 삽입하고 방문처리 => 탐색
      if(!bfs_visited[node]){
        queue.push(node);
        bfs_visited[node] = true;
      }
    }
  }
}

dfs(v);
console.log(dfs_graph.join(' '));
bfs(v);
console.log(bfs_graph.join(' '));
```
