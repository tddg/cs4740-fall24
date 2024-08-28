---
layout: home
title: CS4740, Fall 2024
nav_exclude: true
type: Course
name: CS4740, Cloud Computing
---

# {{ site.title }}: {{ site.tagline }}
{: .mb-2 }
{: .fs-6 .fw-300 }

{% if site.announcements %}
{{ site.announcements.last }}
 [Announcements]({{ site.baseurl }}{% link announcements.md %}){: .btn .btn-outline .fs-3 }
{% endif %}


## Overview

In today’s digital age, cloud computing has fundamentally transformed
how we work, communicate, and interact. By leveraging distributed
systems, it provides flexible and scalable solutions that cater to
various computing demands, from storing vast amounts of data to
running complex applications.

This is an undergraduate-level course which explores system
perspectives of cloud computing, including key concepts and
principles in the design and development of cloud systems. This
course primarily emphasizes the perspective of **system providers**
rather than users. The lectures cover distributed system concepts
such as distributed communication models, synchronization,
consistency, fault tolerance, as well as introduction to influential
cloud infrastructure and frameworks. Students gain practical
experience through the study of algorithms and the project of
building distributed applications.



## Lecture Info

* Instructor: [Yue Cheng](https://tddg.github.io)
* Meeting time: MW 3:30 pm - 4:45pm
* Location: Olsson Hall 005
	* **IMPORTANT:** Initial classes will be conducted online as the instructor is attending an international conference ([VLDB'24](https://vldb.org/2024/)). Students have two options to join the class via: a) classroom streaming (Olsson Hall 005) or b) online Zoom meeting (link posted on Canvas). **Note** the teaching will transition to in-person classes in **Week 3** once the instructor is back on campus. 


## High-level Topics (tentative)

* Introduction
* Fundamentals
* Fault tolerance and consensus
* Big data processing 
* Consistency, scalability, transactions
* Datacenter techniques
* Reasoning about system performance



## Prerequisite

* CS 2130 (Computer Systems and Organization). **Solid system programming skills** are strongly recommended.
	* This is a **programming-intensive** course. You need to be comfortable with programming in one of the following languages including C, Python, Java. Knowing a language serves as a soft prerequisite and would get you prepared to learn a new language (if you haven’t already) and do the programming assignments in this course.

* Being comfortable with programming in one of the aforementioned languages will ease the learning curve for you to pick up a new programming language [Go](https://go.dev/) if you haven't used Go before. All programming assignments will be in Go. 
