---
layout: assignment
title: "Lab 2"
permalink: /labs/lab2
parent: Labs
nav_order: 3
due_date: "Wednesday Oct 16, 11:59pm ET"
---


# RPC and Key-Value Server

## Resources

You can find the video recording of Lab 2 tutorial
[here](https://edstem.org/us/courses/65103/discussion/5397302). The
slides are posted [here](/cs4740-fall24/assets/docs/lab2_tutorial.pdf).

## Introduction

In this lab you will build a key/value server for a single machine
that ensures that each operation is executed exactly once despite
network failures and that the operations are linearizable. Later labs
will replicate a server like this one to handle server crashes.

Clients can send three different RPCs to the key/value server:
`Put(key, value)`, `Append(key, arg)`, and `Get(key)`. The server
maintains an in-memory map of key/value pairs. Keys and values are
strings.  `Put(key, value)` installs or replaces the value for a
particular key in the map, `Append(key, arg)` appends arg to key's
value and returns the old value, and `Get(key)` fetches the current
value for the key. A `Get` for a non-existent key should return an
empty string. An `Append` to a non-existent key should act as if the
existing value were a zero-length string. Each client talks to the
server through a Clerk with Put/Append/Get methods. A `Clerk` manages
RPC interactions with the server.

Your server must arrange that application calls to `Clerk`
`Get`/`Put`/`Append` methods be linearizable. If client requests aren't
concurrent, each client `Get`/`Put`/`Append` call should observe the
modifications to the state implied by the preceding sequence of
calls. For concurrent calls, the return values and final state must
be the same as if the operations had executed one at a time in some
order. Calls are concurrent if they overlap in time: for example, if
client X calls `Clerk.Put()`, and client Y calls `Clerk.Append()`, and
then client X's call returns. A call must observe the effects of all
calls that have completed before the call starts.

Linearizability is convenient for applications because it's the
behavior you'd see from a single server that processes requests one
at a time. For example, if one client gets a successful response from
the server for an update request, subsequently launched reads from
other clients are guaranteed to see the effects of that update.
Providing linearizability is relatively easy for a single server. 


## Getting Started

We supply you with skeleton code and tests in `src/kvsrv`. You will
need to modify `kvsrv/client.go`, `kvsrv/server.go`, and
`kvsrv/common.go`.

To get up and running, download the software from 
[here](https://tddg.github.io/cs4740-fall24/assets/lab_code/lab2.tar). 

```sh
% cd $HOME
% wget https://tddg.github.io/cs4740-fall24/assets/lab_code/lab2.tar
% tar -xvf lab2.tar
% ls
src/
```

You should copy all subdirs from the `src/` dir to an existing dir
`cs4740-fall24-labs/src`, which you have created for Lab 1. To do so:

```sh
% cp -r src/* cs4740-fall24-labs/src/
% cd cs4740-fall24-labs/src/
% ls
kvsrv  labgob  labrpc  models  porcupine
```


Now, `src/` should contain seven dirs, out of which `kvsrv/`,
`labgob/`, `labrpc/`, `models/`, and `porcupine/` are for Lab 2.


## Part 1: Basic key-value server with no network failures


Your first task is to implement a solution that works when there are
no dropped messages.

You'll need to add RPC-sending code to the Clerk `Put`/`Append`/`Get`
methods in `client.go`, and implement `Put`, `Append()` and `Get()` RPC
handlers in `server.go`.

You have completed this task when you pass the first two tests in the
test suite: "one client" and "many clients". 

> **Hint:** Check that your code is race-free using `go test -race`.


## Part 2: Key-value server with dropped messages

Now you should modify your solution to continue in the face of
dropped messages (e.g., RPC requests and RPC replies). If a message
was lost, then the client's `ck.server.Call()` will return false (more
precisely, `Call()` waits for a reply message for a timeout interval,
and returns false if no reply arrives within that time). One problem
you'll face is that a `Clerk` may have to send an RPC multiple times
until it succeeds. Each call to `Clerk.Put()` or `Clerk.Append()`,
however, should result in just a single execution, so you will have
to ensure that the re-send doesn't result in the server executing the
request twice. 

In this part, you should add code to `Clerk` to retry if it doesn't
receive a reply, and to `server.go` to filter duplicates if the
operation requires it.  

> * **Hint:** You will need to uniquely identify client operations to ensure that the key/value server executes each one just once. 
> * **Hint:** You will have to think carefully about what state the server must maintain for handling duplicate `Get()`, `Put()`, and `Append()` requests, if any at all. 
> * **Hint:** Your scheme for duplicate detection should free server memory quickly, for example by having each RPC imply that the client has seen the reply for its previous RPC. It's OK to assume that a client will make only one call into a Clerk at a time. 

Your code should now pass all tests, like this: 

```sh
go test
Test: one client ...
  ... Passed -- t  3.8 nrpc 31135 ops 31135
Test: many clients ...
  ... Passed -- t  4.7 nrpc 102853 ops 102853
Test: unreliable net, many clients ...
  ... Passed -- t  4.1 nrpc   580 ops  496
Test: concurrent append to same key, unreliable ...
  ... Passed -- t  0.6 nrpc    61 ops   52
Test: memory use get ...
  ... Passed -- t  0.4 nrpc     4 ops    0
Test: memory use put ...
  ... Passed -- t  0.2 nrpc     2 ops    0
Test: memory use append ...
  ... Passed -- t  0.4 nrpc     2 ops    0
Test: memory use many puts ...
  ... Passed -- t 11.5 nrpc 100000 ops    0
Test: memory use many gets ...
  ... Passed -- t 12.2 nrpc 100001 ops    0
PASS
ok      6.5840/kvsrv    41.000s
```

The numbers after each `Passed` are real time in seconds, number of
RPCs sent (including client RPCs), and number of key/value operations
executed (`Clerk` `Get`/`Put`/`Append` calls). 



## Point distribution

For this lab, each test is worth 5 points. If your code passes all
tests, you will get 45 points.  Your code will be tested on Autolab.
No marks will be awarded if your code does not pass the test. You
will receive full marks only if your code successfully passes the
test.


## What (and how) to hand in


### Submitting on Autolab

You must turn in your lab assignment using
[Autolab](http://autolab-cs4740.com/).  Read this
[document](https://docs.google.com/document/d/1G_fpExlF6k4LtUF2reqAm8WI73wUSlvt0iX0ZrXZGBA/edit#heading=h.qkqs78p5a2np) 
for instructions on how to sign-up for Autolab. If you did not
receive a confirmation email from Autolab to set a password, enter
your @gmu.edu (**NOT** masonlive) email, and click "Forgot your
password" to get a new password. You may skip the Autolab signup 
step if you have done this in lab0.

Create a **tar** file of only the following Go source files,
`client.go`, `common.go`, and `server.go`.  Please, .tar only, not
.tgz, nor .7z/.zip. Name your tar file as `lab2-handin.tar`. 


```sh
% tar -cvf lab2-handin.tar client.go common.go server.go
```

**Please do not put any directory in your tar file
as our autograder is scripted to directly fetch src files not
directories.** Use the following command to examine the content 
of your tar file:

```sh
% tar -tvf lab2-handin.tar
```

When you upload your lab, Autolab will automatically untar it
and test it. You should verify that the result that Autolab generates
is what you expect. Test your code locally before submitting it to
Autolab.  Your code is tested in a cloud Linux VM. Assignments that
do not compile or run will receive a maximum of 50%. Note that we
have provided ample resources for you to verify that our view of your
assignment is the same as your own: you will see the result of the
test execution for your assignment when you submit it. 

You can resubmit your assignment an unlimited number of times before
the deadline. Note the late submission policy: assignments will be
accepted up until 3 days past the deadline at a penalty of 10% per
late day; after 3 days, no late assignments will be accepted, no
exceptions.


### Submitting on Canvas

Please also submit your tar file on Canvas. The submission should
include the three Go source files, and this is just for the GTAs'
record.

> **NOTE:** Canvas submission will remain open after the due date,
> but we will not use Canvas submission timestamp. **Instead, we will
> use the timestamp of your last Autolab submission for late penalty.**



## Acknowledgment

The lab assignment is adapted from MIT's 6.824 course. Thanks to
Frans Kaashoek, Robert Morris, and Nickolai Zeldovich for their
support.


