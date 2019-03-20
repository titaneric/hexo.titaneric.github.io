---
title: about
date: 2019-03-20 12:27:55
type: "about"
---

# Background

Graduate Student at National Chiao Tung University, Taiwan
Major in Data Science

# Skill tree

## Fundamental skills

```graphviz
digraph hierarchy {
    node [color=Black,fontname=consolus,shape=oval] //All nodes will this shape and colour
    "Fundamental skills"->"Programming language"
    "Programming language"->"Python 3"
    "Programming language"->"C++ 11"
    "Programming language"->"ES6"
    
    "Fundamental skills"->"Git"
    "Git"->"Git flow"
}
```

## Web Development

```graphviz
digraph hierarchy {
    node [color=Black,fontname=consolus,shape=oval] //All nodes will this shape and colour
    subgraph Web {
        "Web"->"frontend"
        "backend"->"Web"[dir=back]
    }
    subgraph frontend {
        "frontend"->"Vue"
        "Vue"->"Vuex"
        
        "frontend"->"ES6"
        "ES6"->"airbnb"
   }
    subgraph backend {
        "backend"->"framework"
        "framework"->"Laravel"
        "Laravel"->"PSR-2"
        
        "framework"->"Django"
        "Django"->"PEP8"
        
        "backend"->"database"
        "database"->"MySQL"
        "database"->"MongoDB"
        
        "backend"->"cache"
        "cache"->"redis"
        
        "backend"->"RESTful APIs"
        
        "backend"->"crawler"
    }
}
```

## Data Science

```graphviz
digraph hierarchy {
    node [color=Black,fontname=consolus,shape=oval] //All nodes will this shape and colour
    subgraph data{
        "Data Science"->"Machine Learning"
        "Deep Learning"->"Data Science"[dir=back]
        "Data Science"->"Data Preprocessing"
        "Data Science"->"Math"
    }
    subgraph math{
        "Linear Algebra"->"Math"[dir=back]
        "Statistics"->"Math"[dir=back]
        "Math"->"Probability"
        "Math"->"Convex Optimization"
    }
    subgraph preprocess{
        "Data Preprocessing"->"text"
        "Data Preprocessing"->"audio"
        "Data Preprocessing"->"image"
    }
    subgraph textPre{
        "text"->"awk"
        "text"->"numpy"
        "text"->"pandas"
    }
    subgraph audioPre{
        "audio"->"ffmpeg"
    }
    subgraph imagePre{
        "image"->"OpenCV"
    }
}
```

## Experience

### Course

- OSDI
- Data Mining

### Project

- [Account System](https://account.cs.nctu.edu.tw/)
- Room Reservation System
- Visualization 119 in Taiwan
- Data visulization
- EZMusix