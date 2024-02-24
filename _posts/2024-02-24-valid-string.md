---
layout: post
title: Valid Number
categories: [Strings,DFA]
tags: [valid number,leetcode,interviewbit]
---

Validate if a given string can be interpreted as a decimal number.

**[Go to Problem](https://leetcode.com/problems/valid-number/)**

```bash
Some examples:
"0" => true
" 0.1 " => true
"abc" => false
"1 a" => false
"2e10" => true
" -90e3   " => true
" 1e" => false
"e3" => false
" 6e-1" => true
" 99e2.5 " => false
"53.5e93" => true
" --6 " => false
"-+3" => false
"95a54e53" => false
```

__Note:__ 

It is intended for the problem statement to be ambiguous.<br/>
You should gather all requirements up front before implementing one.<br/> 
However, here is a list of characters that can be in a valid decimal number:

1. Numbers 0-9
1. Exponent - "e"
1. Positive/negative sign - "+"/"-"
1. Decimal point - "."

### <span style="color:#e74c3c">Solution:</span>
---
__Approach: DFA__
* Time Complexity:  O(n)
* Space Complexity: O()

Lets Represent the current problem using DFA:
 
![DFA][DFA_image]

[DFA_image]: {{ site.BASE_PATH }}/assets/media/valid_number_dfa_leetcode.png "Logo Title Text 2"

__1.__ How do we represent the states set in code(c++) ?

    Using list of hash maps, where hash maps will store the transitions.
```cpp
vector<unordered_maps<string,int>> states;
```
    Transitions from a state are stored in a map:   
```cpp
{ //State 1
    unordered_map<string,int> map;
    map.insert({"space", 1});
    map.insert({"sign" , 2});
    map.insert({"digit", 3});
    map.insert({".",4});
    //push the state in the states vector
    states.push_back(map);
}
```
    Here in state 1 there are 4 transitions.
    A transition is represented using key value pair:
    {"input_string",next_state}. 

__2. Algorithm:__

    Start from current_state = 1
    For each char in the input string:
    
        a. Map the char to valid Alphabets!
        * [0-9] -> digit
        * +/-   -> sign
        * " "   -> space
        
        b. Check if for the current_state has
        transition for the current i/p.
        
        return false if not present.

        c. If present move to next state.
    
        d. After all input char has been scanned,
        check if the state we ended is final state or not!.
        if not return false else return true.

__C++ Implementation:__
```cpp
bool isNumber(string s) {
    
    vector<unordered_map<string,int>> states;
    
    { // State 0
        unordered_map<string,int> map;
        states.push_back(map);
    }
    
    { //State 1
        unordered_map<string,int> map;
        map.insert({"space", 1});
        map.insert({"sign" , 2});
        map.insert({"digit", 3});
        map.insert({".",4});
        states.push_back(map);
    }
    
    { //State 2
        unordered_map<string,int> map;
        map.insert({"digit", 3});
        map.insert({".", 4});
        states.push_back(map);
    }
    
    { //State 3
        unordered_map<string,int> map;
        map.insert({"digit", 3});
        map.insert({".",5});
        map.insert({"e",6});
        map.insert({"space",9});
        states.push_back(map);
    }
    
    { //State 4
        unordered_map<string,int> map;
        map.insert({"digit", 5});
        states.push_back(map);
    }
    { //State 5
        unordered_map<string,int> map;
        map.insert({"digit", 5});
        map.insert({"e",6});
        map.insert({"space",9});
        states.push_back(map);
    }
    
    { //State 6
        unordered_map<string,int> map;
        map.insert({"digit", 8});
        map.insert({"sign", 7});
        states.push_back(map);
    }
    { //State 7
        unordered_map<string,int> map;
        map.insert({"digit",8});
        states.push_back(map);
    }
    { //State 8
        unordered_map<string,int> map;
        map.insert({"digit", 8});
        map.insert({"space", 9});
        states.push_back(map);
    }
    
    { //State 9
        unordered_map<string,int> map;
        map.insert({"space",9});
        states.push_back(map);
    }
        
        
    unordered_map<int,int> final_state;
    final_state.insert({3,0});
    final_state.insert({5,0});
    final_state.insert({8,0});
    final_state.insert({9,0});
    
    int currentState = 1;
    for(auto c: s){
        // a. map the char to valid alphabets of ourdfa
        string char_s = "";
        if(isdigit(c)){
            char_s = "digit";
        }else if(c==' '){
            char_s = "space";
        }else if(c=='+' ||c =='-'){
            char_s ="sign";
        }else{
            char_s = c;
        }
        //b. check if the i/p is present in currentstate
        if(states[currentState].find(char_s) ==states[currentState].end()){
            return false;   //if not present returnfalse
        }
        //c. if present go to next state
        currentState = states[currentState].fin(char_s)->second;
        
    }
    //d. check if the ended state is final state
    if (final_state.find(currentState) == final_state.end()){
            return false;  //if not return false
    }
        //if yes in one of the final state return true 
    return true;
}
```
