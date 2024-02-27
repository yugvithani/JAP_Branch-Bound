# JAPvis_Branch&Bound
## Job Sequencing Problem Visualization using Branch & Bound
### Overview
This project implements the Branch and Bound algorithm to Visualize and to solve the Job Sequencing Problem, a classic optimization problem in computer science and operations research. The Job Sequencing Problem involves scheduling a job to a employee to minimize a total completion time.

### Problem
Given a set of jobs and a set of workers(employees) in form of matrix which cell show the completion time of a job by a worker. The objective is to schedule the jobs to a unemployeed worker in way that minimize the total compltion time. Each worker do only one job.

### Implementation using C++
```
#include<bits/stdc++.h>
using namespace std;

class Node{
    public:
    Node* par; //for print solution
    int cost; //actual cost + estimate cost
    int job;
    int emp;
    int pathcost; //actual cost
    vector<bool> assign;   

    Node(Node* p, int j, int e, vector<bool> a, int n){
        par=p;
        job=j;
        emp=e;
        assign=a;
    } 
};

class Compare{
    public:
    bool operator()(Node* l, Node* r){
        return l->cost > r->cost;
    }
};

int clac_cost(int **t, int job, int emp, vector<bool> assign, int n){
    int cost=0, i, j;

    for(i=emp+1; i<n; i++){
        int min=1e9;

        for(j=0; j<n; j++){
            if(!assign[j] && t[i][j]<min){   
                min=t[i][j];
            }
        }
        cost+=min;
    }
    return cost;
}

void print(Node* min){
    if(min->par == NULL)
        return;
    print(min->par);
    cout<<"Employee "<<min->emp +1 <<" have Job "<<min->job +1<<"."<<endl;
}

int main(){
    int n, i, j;
    cin>>n;
    int **t=new int*[n];
    for(i=0; i<n; i++)
    {
        t[i]=new int[n];
        for(j=0; j<n; j++)
            cin>>t[i][j];
    }
    
    priority_queue<Node*, vector<Node*>, Compare> pq;
    vector<bool> assign(n, false);
    Node* root=new Node(NULL, -1, -1, assign, n);
    root->cost=root->pathcost=0;

    pq.push(root);

    while(!pq.empty()){
        Node* min=pq.top();
        pq.pop();

        int i=min->emp + 1;
        if(i==n){
            print(min);
            cout<<endl<<min->cost;
            exit(0);
        }
        int j;
        for(j=0; j<n; j++){
            if(!min->assign[j]){
                vector<bool> new_assign = min->assign;
                new_assign[j] = true;
                Node* child=new Node(min, j, i, new_assign, n);
                child->pathcost=min->pathcost + t[i][j];
                child->cost = child->pathcost + clac_cost(t, j, i, new_assign, n);
                pq.push(child);
            }
        }
    }
}
```

### Example
Given 2D Matrix's dimensions is worker * job.
Input
```
4
11 12 18 40
14 15 13 22
11 17 19 23
17 14 20 28
```

Output
```
Employee 1 have Job 1.
Employee 2 have Job 3.
Employee 3 have Job 4.
Employee 4 have Job 2.

61
```
Here 11 + 13 + 23 + 14 = 61 is result by shown in output.

## Contributers & Special Thanks to
Vrund Dobariya...... https://github.com/VGD3626 <br>
Kishan Dervaliya.... https://github.com/Kdenius <br>
Divyang Dhameliya... https://github.com/Divyang029 <br>
Yug Vithani......... https://github.com/yugvithani <br>

