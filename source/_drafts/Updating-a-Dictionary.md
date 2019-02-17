---
title: Updating-a-Dictionary
tags:
 - problem
 - c++
---

```c++
#include <iostream>
#include <map>
#include <algorithm>
#include <string>
#include <regex>
#include <vector>
#include <set>
#include <iterator>

using namespace std;

regex key_regex("([a-z]+)");
regex value_regex("([0-9]+)");

void parse2Dict(string &line, map<string, string> &dict)
{
    auto key_begin = sregex_iterator(line.begin(), line.end(), key_regex);
    auto key_end = sregex_iterator();

    auto value_begin = sregex_iterator(line.begin(), line.end(), value_regex);
    auto value_end = sregex_iterator();
    for (auto k = key_begin, v = value_begin; k != key_end && v != value_end; ++k, ++v)
    {
        dict[k->str()] = v->str();
    }
}
void output(char op, set<string> &target)
{
    vector<string> target_vec(target.begin(), target.end());
    cout << op;
    for (auto k : target_vec)
    {
        cout << k;
        if (k != target_vec.back())
            cout << ",";
    }
    cout << endl;
}
void compareDict(map<string, string> &previous, map<string, string> &current)
{
    if (previous == current)
    {
        printf("No changes\n");
    }
    else
    {
        set<string> previous_key;
        for (auto iter : previous)
        {
            previous_key.insert(iter.first);
        }

        set<string> current_key;
        for (auto iter : current)
        {
            current_key.insert(iter.first);
        }

        // *
        set<string> possible_star;
        set_intersection(current_key.begin(), current_key.end(), previous_key.begin(), previous_key.end(), inserter(possible_star, possible_star.begin()));

        // +
        set<string> plus;
        set_difference(current_key.begin(), current_key.end(), possible_star.begin(), possible_star.end(), inserter(plus, plus.begin()));
        if (plus.size())
            output('+', plus);

        // -
        set<string> minus;
        set_difference(previous_key.begin(), previous_key.end(), possible_star.begin(), possible_star.end(), inserter(minus, minus.begin()));
        if (minus.size())
            output('-', minus);

        // *
        set<string> star;
        copy_if(possible_star.begin(), possible_star.end(), inserter(star, star.begin()), [&](string k) { return current[k] != previous[k]; });
        if (star.size())
            output('*', star);
    }
}
int main()
{
    int n;
    string line;
    scanf("%d", &n);
    getline(cin, line);

    for (int i = 0; i < n; i++)
    {
        getline(cin, line);
        map<string, string> previous_map;
        parse2Dict(line, previous_map);

        getline(cin, line);
        map<string, string> current_map;
        parse2Dict(line, current_map);

        compareDict(previous_map, current_map);
        cout << endl;
    }

    return 0;
}
```
