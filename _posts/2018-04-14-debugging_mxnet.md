---
title: 'If we debug codes in MxNet'
date: 2018-04-14
permalink: /posts/2018/04/debugging_mxnet/
tags:
  - github
  - Python
---
What should we do for debugging codes in MxNet

## Debugging after you find a bug
- Fork the MxNet reposiotry, and git clone the repository into your PC.
- Create a new branch for your debugging
- The debugged code needs testing. 
  - You would find codes for testing at
  [https://github.com/apache/incubator-mxnet/blob/master/tests/](https://github.com/apache/incubator-mxnet/blob/master/tests/)  
  - The code does not cosider the bug that you are solving; the bug was not detected by the test. So you need to change the codes for testing to validate your debugging.
  - Run the changed code by
  ``` nosetests -v path_to_the_test ```
- Commit & push to your repository. The necessary files are your debugging codes and testing codes.

## Sending a pull request to MxNet repository
- Before sending a pull request, cut a JIRA ticket about the bug on [https://issues.apache.org/jira/projects/MXNET](https://issues.apache.org/jira/projects/MXNET)
- Then the ticket ID like [MXNET-***] is issued. 
- Make a pull request with a title including [MXNET-***]
