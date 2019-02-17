---
title: Searching-the-Web
tags:
 - problem
 - c++
---

```c++
#include <iostream>
#include <map>
#include <regex>
#include <set>
#include <vector>
#include <utility>
#include <string>
#include <algorithm>
#include <iterator>

using namespace std;

regex word_regex("([a-zA-Z]+)");
void tokenized(string &line, int did, int lino, map<string, set<pair<int, int>>> &inverted_map, map<string, set<int>> &article_map)
{
    auto words_begin = sregex_iterator(line.begin(), line.end(), word_regex);
    auto words_end = sregex_iterator();

    for (auto i = words_begin; i != words_end; ++i)
    {
        string match_str = i->str();
        transform(match_str.begin(), match_str.end(), match_str.begin(), ::tolower);
        article_map[match_str].insert(did);
        inverted_map[match_str].insert(make_pair(did, lino));
    }
}

vector<string> getMultipleRequirement(string &inst)
{
    regex req_regex("([a-z]+)");
    auto words_begin = sregex_iterator(inst.begin(), inst.end(), req_regex);
    auto words_end = sregex_iterator();

    vector<string> req;
    for (auto i = words_begin; i != words_end; ++i)
    {
        string match_str = i->str();
        req.push_back(match_str);
    }
    return req;
}
void outputEndLine(char c)
{
    for (int i = 0; i < 10; i++)
        cout << c;
    cout << endl;
}
void outputResult(set<int> &result, vector<pair<int, int>> &refs, vector<vector<string>> &articles)
{
    if (result.size())
    {
        set<pair<int, int>> refs_set = set<pair<int, int>>(refs.begin(), refs.end());
        vector<int> vec(result.begin(), result.end());
        for (auto did : vec)
        {
            for (auto p : refs_set)
            {
                if (p.first == did)
                {
                    cout << articles[p.first - 1][p.second - 1] << endl;
                }
            }
            if (did != vec.back())
                outputEndLine('-');
        }
    }
    else
    {
        cout << "Sorry, I found nothing.\n";
    }
    outputEndLine('=');
}
// output for NOT
void outputResult(set<int> &result, vector<vector<string>> &articles)
{
    if (result.size())
    {
        vector<int> vec(result.begin(), result.end());
        for (auto did : vec)
        {
            for (auto line : articles[did - 1])
            {
                cout << line << endl; 
            }
            if (did != vec.back())
                outputEndLine('-');
        }
    }
    else
    {
        cout << "Sorry, I found nothing.\n";
    }
    outputEndLine('=');
}
vector<pair<int, int>> getValue(map<string, set<pair<int, int>>> &dict, string key)
{
    auto iter = dict.find(key);
    if (iter == dict.end())
    {
        return vector<pair<int, int>>({});
    }
    return vector<pair<int, int>>(iter->second.begin(), iter->second.end());
}
set<int> getValue(map<string, set<int>> &dict, string key)
{
    auto iter = dict.find(key);
    if (iter == dict.end())
    {
        return set<int>({});
    }
    return iter->second;
}
int main()
{
    int n;
    string line;
    scanf("%d", &n);
    getchar();

    vector<vector<string>> articles;

    map<string, set<int>> article_map;
    map<string, set<pair<int, int>>> inverted_map;
    vector<string> article;
    while (true)
    {
        getline(cin, line);
        if (line[0] == '*')
        {
            articles.push_back(article);
            article.clear();
            if (articles.size() == n)
            {
                break;
            }
        }
        else
        {
            article.push_back(line);
            tokenized(line, articles.size() + 1, article.size(), inverted_map, article_map);
        }
    }

    int k = 1;
    vector<int> total(n);
    generate(total.begin(), total.end(), [k]() mutable { return k++; });
    set<int> total_set(total.begin(), total.end());

    int m;
    scanf("%d", &m);
    getchar();

    for (int i = 0; i < m; i++)
    {
        getline(cin, line);

        vector<string> reqs = getMultipleRequirement(line);

        if (line.find("AND") != string::npos && reqs.size() == 2)
        {
            vector<pair<int, int>> first_vec = getValue(inverted_map, reqs[0]);
            vector<pair<int, int>> second_vec = getValue(inverted_map, reqs[1]);
            vector<pair<int, int>> total(first_vec);
            total.insert(total.end(), second_vec.begin(), second_vec.end());

            set<int> first_did = getValue(article_map, reqs[0]);
            set<int> second_did = getValue(article_map, reqs[1]);
            set<int> inter;
            set_intersection(first_did.begin(), first_did.end(), second_did.begin(), second_did.end(), inserter(inter, inter.begin()));
            outputResult(inter, total, articles);
        }
        else if (line.find("OR") != string::npos && reqs.size() == 2)
        {
            vector<pair<int, int>> first_vec = getValue(inverted_map, reqs[0]);
            vector<pair<int, int>> second_vec = getValue(inverted_map, reqs[1]);
            vector<pair<int, int>> total(first_vec);
            total.insert(total.end(), second_vec.begin(), second_vec.end());

            set<int> first_did = getValue(article_map, reqs[0]);
            set<int> second_did = getValue(article_map, reqs[1]);
            set<int> union_set;
            set_union(first_did.begin(), first_did.end(), second_did.begin(), second_did.end(), inserter(union_set, union_set.begin()));
            outputResult(union_set, total, articles);
        }
        else if (line.find("NOT") != string::npos && reqs.size() == 1)
        {
            set<int> target = getValue(article_map, reqs[0]);
            set<int> diff_set;
            set_difference(total_set.begin(), total_set.end(), target.begin(), target.end(), inserter(diff_set, diff_set.begin()));
            outputResult(diff_set, articles);
        }
        else if (reqs.size() == 1)
        {
            vector<pair<int, int>> refs = getValue(inverted_map, reqs[0]);
            set<int> results = getValue(article_map, reqs[0]);
            outputResult(results, refs, articles);
        }
    }

    return 0;
}
```
