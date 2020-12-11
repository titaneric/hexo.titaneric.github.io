---
title: About myself
date: 2019-03-20 12:27:55
type: "about"
---

## Profile

Persistent learner, dedicated graduate who major in data science. Skilled in data wrangling, machine learning knowledge and data visualization. Strong in Python and experienced with large-scale software code reading and tracing (E.g., CPython, [PyTorch](\https://github.com/pytorch/pytorch/pull/28651) & [Buddy System in Linux kernel](https://titaneric.github.io/2019/09/23/Buddy-system/)).

## Experience

### Work

- CIM Automation Engineer at tsmc [Oct 2020-Present]
  - Developed the general resume parser and achieved up to *95%* item accuracy
  - Wrote unit-test, documents for prior's project.
- Web Developer at NCTU CSCC [July 2018–Jan 2019]
  - Developed the web service, especially for [Account Application System](https://account.cs.nctu.edu.tw/).
  - Implemented in Laravel and Vue frameworks.
  - Applied Git flow on development and Gitlab runner to automate the test and deployment jobs.

### Open Source Contribution

- Found the redundant calculation of derivative of power function across various deep learning framework.
  - [PyTorch](https://github.com/pytorch/pytorch/pull/28651)
  - [JAX](https://github.com/google/jax/pull/1578)
  - [Autograd](https://github.com/HIPS/autograd/pull/541)
- Improved and beautified one of the example
  - [Yew](https://github.com/yewstack/yew/pull/1650)
- Developed some code to be more Pythonic
  - [TensorFlow](https://github.com/tensorflow/tensorflow/pull/32126)
- Bug reporting
  - [Python extension for Visual Studio Code](https://github.com/microsoft/vscode-python/issues/202)
- Participation of issue discussion
  - [Windows Subsystem for Linux](https://github.com/MicrosoftDocs/WSL/issues/404#issuecomment-504759326)

### Project

- Real-time Traffic Anomaly Detector
  - Anomaly detection in NCTU administration networks.
  - Designed and implemented the data pre-processing pipeline with Apache Kafka, Spark and MongoDB.
  - Processed data-stream in real time up to **30 kB/sec** and transformed into feature vectors to further predict
- Music Recommendation System
  - Recommendation System using KKBox WSDM data
  - Collected the additional data from Spotify and preprocessing them.
  - Implemented the web-based interface to visualize the recommendation and user preference.
- [AutoDiff from scratch](https://github.com/titaneric/AutoDiff-from-scratch)
  - Simple neural network library supporting auto-differentiation
  - Reported an issue and solve it in various deep-learning library during the development.
  - Enabled high-level layer usage and have already tested on real-world dataset.
- [Account Application System](https://account.cs.nctu.edu.tw/)
  - Web service for NCTU CS students to apply their account
  - Developed in Laravel MVC architecture  with additional Repository pattern.
  - Designed the database schema.
  - Implemented the business logic and account-activation-status API.

## Skills

### Data Science

- Python
  - Pandas
  - NumPy
  - PyTorch
- Data Visualization
- Data Preprocessing

Greatly influenced from [how data science resume cv](https://www.dataquest.io/blog/how-data-science-resume-cv/).

### Web

- Web framework
  - Laravel
- Frontend framework
  - Vue
- Database
  - MySQL
  - Transact-SQL
  - MongoDB
- API Design
- Test
  - PHPUnit
  - Jest
- CI/CD
  - Travis
  - Gitlab

Greatly influenced from [developer-roadmap](https://github.com/kamranahmedse/developer-roadmap).

## Resume

### English version

{% pdf ./eg_resume.pdf
%}

### Chinese version

{% pdf ./tw_resume.pdf
%}


## Biography

{% pdf ./bio.pdf
%}

Last Updated: 2020/08/27