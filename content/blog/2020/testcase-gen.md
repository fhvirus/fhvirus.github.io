---
title: "FHVirus' Testcase Generator"
date: 2020-11-16T19:44:44+08:00
draft: false
description: "剛好要出模競題就順便寫了一個生測資的東東，希望有用><"
tags: [
    "模板",
]
---

剛好要出模競題就順便寫了一個生測資的東東，希望有用><

<!--more-->

說明都在裡面了，自己看吧 OW0)\_b\
至於為什麼是英文的？因為我不知道怎麼樣在 Vim 裡面打中文 QwQ\
有人會的話麻煩教我（我用的是 Mac / zsh ）

```cpp
/*           OJDL TestCase Generater             */
/*  Made by FHVirus with patience for cin/cout   */
/*
How this works:
1.  Generate a file (testcase#.in)
2.  Read that file as input and solve,
    output to another file(testcase#.out)

How to use it:
0.  Put this program into a folder where
    you want to store the testcases.
1.  Input number of subtasks
2.  For each subtask, input the number of
    desired testcase for the subtask, then
    the arguments for generate().
3.  Done!
*/
#include<iostream>
#include<fstream>
#include<string>
#include<chrono>
#include<random>
using namespace std;

auto seed = std::chrono::high_resolution_clock::now().time_since_epoch().count();
std::mt19937 mt(seed);

// For generate(), you can add additional arguments
// for generating a testcase.
// But for solve(), just write a normal solution
// like that in main() for most of the time.
void generate(ofstream &cout){}
void solve(ifstream &cin, ofstream &cout){}

int main(){
    int subtaskNumber, testcaseCount = 1;
    cin >> subtaskNumber;
    for(int t = 1; t <= subtaskNumber; ++t){
        int numOfTestCase;
        cin >> numOfTestCase;
        // You can add some other arguments for
        // generating testcases.

        cout << "Generating Subtask #" << t << '\n';
        for(; numOfTestCase; --numOfTestCase, ++testcaseCount){
            string in = to_string(testcaseCount) + ".in";
            string ou = to_string(testcaseCount) + ".out";
            {
                ofstream tsezi;
                tsezi.open(in);
                // Remember to add arguments here too.
                generate(tsezi);
                tsezi.close();
            }
            {
                ifstream tsezi;
                ofstream da_an;
                tsezi.open(in);
                da_an.open(ou);
                solve(tsezi, da_an);
                tsezi.close();
                da_an.close();
            }
        }
    }
    return 0;
}
```
