---
title: Concurrency-Simulator
tags:
 - problem
 - c++
---

```c++
#include <iostream>
#include <queue>
#include <deque>
#include <string>
#include <algorithm>
#include <vector>
#include <sstream>

using namespace std;

enum State
{
    TERMINATED,
    REMAINED,
    EXECUTED
};

class OperatingSystem
{
    class Process
    {
        vector<string> instructions;
        int instruction_point;
        const int *delta;
        char variable;
        int value;
        const int default_value = 100;
        const char default_variable = ' ';

      public:
        int pid;
        State state;
        int current_instruction;
        int remaining_quantum;

      public:
        Process(int pid, const int *delta) : pid(pid), instruction_point(0), remaining_quantum(0), state(State::EXECUTED), current_instruction(-1), delta(delta)
        {
            this->load_instructions();
        }

      public:
        const string *fetch()
        {
            if (state == State::EXECUTED)
            {
                return &instructions[instruction_point++];
            }
            return NULL;
        }
        void execute(OperatingSystem *os)
        {
            switch (this->current_instruction)
            {
            case 0: // assign
                os->assign_variable(this->variable, this->value);
                break;
            case 1: // print
                os->print_variable(this->variable, this->pid);
                break;
            case 2: // lock
                os->lock(this);
                break;
            case 3: // unlock
                os->unlock(this);
                break;
            case 4: // end
                state = State::TERMINATED;
                break;
            default:
                break;
            }
        }
        void decode()
        {
            const string *instr = this->fetch();
            if (instr == NULL)
                return;
            // cout << this->pid << ": " << *instr << endl;
            string instr_prefix = instr->substr(0, 2);
            this->value = this->default_value;
            this->variable = this->default_variable;

            if (instr_prefix == "lo")
            {
                this->current_instruction = 2;
            }
            else if (instr_prefix == "un")
            {
                this->current_instruction = 3;
            }
            else if (instr_prefix == "pr")
            {
                this->current_instruction = 1;
                this->variable = *find_if(instr->rbegin(), instr->rend(), [](char c) { return isalpha(c); });
            }
            else if (instr_prefix == "en")
            {
                this->current_instruction = 4;
            }
            else
            {
                this->current_instruction = 0;
                this->variable = *find_if(instr->begin(), instr->end(), [](char c) { return isalpha(c); });
                stringstream ss(instr->substr(instr->find('=') + 1));
                ss >> this->value;
            }
            this->remaining_quantum = this->delta[this->current_instruction];
        }
        void update_quantum(int delta)
        {
            this->remaining_quantum -= delta;
            if (this->remaining_quantum)
            {
                this->state = State::REMAINED;
            }
            else
            {
                this->state = State::EXECUTED;
            }
        }
        void undo_instruction()
        {
            instruction_point--;
        }

      private:
        void load_instructions()
        {
            string line;
            while (true)
            {
                getline(cin, line);
                instructions.push_back(line);
                if (line.substr(0, 3) == "end")
                {
                    break;
                }
            }
        }
    };
    vector<Process *> processes;
    int delta[5];
    int variables[26] = {};
    int quantum;
    int locked_pid;
    deque<Process *> ready_queue;
    queue<Process *> blocked_queue;

  public:
    OperatingSystem() : locked_pid(0)
    {
        this->init();
        this->simulate();
    }

  public:
    void assign_variable(char variable, int value)
    {
        this->variables[variable - 'a'] = value;
    }
    void print_variable(char variable, int pid)
    {
        printf("%d: %d\n", pid, this->variables[variable - 'a']);
    }
    void lock(Process *process)
    {
        if (locked_pid == 0)
        {
            locked_pid = process->pid;
        }
        else
        {
            blocked_queue.push(process);
        }
    }
    void unlock(Process *process)
    {
        if (locked_pid == process->pid)
        {
            locked_pid = 0;
            if (!blocked_queue.empty())
            {
                ready_queue.push_front(blocked_queue.front());
                blocked_queue.pop();
            }
        }
    }

  private:
    void init()
    {
        int n;
        cin >> n;
        processes.resize(n);

        for (int i = 0; i < 5; i++)
        {
            cin >> delta[i];
        }
        cin >> quantum;

        string line;
        getline(cin, line);

        for (int i = 0; i < n; i++)
        {
            processes[i] = new Process(i + 1, delta);
            this->ready_queue.push_back(processes[i]);
        }
    }
    void simulate()
    {
        while (!this->ready_queue.empty())
        {
            Process *process = this->ready_queue.front();
            this->ready_queue.pop_front();

            int allowed_time = this->quantum;
            bool is_locked_process = false;

            while (allowed_time > 0 && process->state != State::TERMINATED)
            {
                process->decode();
                if (process->current_instruction == 2 && locked_pid)
                {
                    process->execute(this);
                    process->undo_instruction();
                    is_locked_process = true;
                    break;
                }
                else
                {
                    int execute_time = process->remaining_quantum;
                    if (execute_time)
                    {
                        allowed_time -= execute_time;
                        // cout << allowed_time << "-" << execute_time << endl;
                        process->update_quantum(execute_time);
                        if (process->state == State::EXECUTED)
                        {
                            process->execute(this);
                        }
                    }
                }
            }

            if (!is_locked_process && process->state != State::TERMINATED)
            {
                this->ready_queue.push_back(process);
            }
        }
    }
};
int main()
{
    int n;
    string line;
    cin >> n;
    getline(cin, line);
    for (int i = 0; i < n; i++)
    {
        OperatingSystem os;
        if (i != (n - 1))
        {
            getline(cin, line); // \n
            cout << endl;
        }
    }
    return 0;
}

```
