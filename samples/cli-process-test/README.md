# Overview

With the WorkflowGen CLI, you can run test scenarios on your processes. Using a definition file, you'll be able to quickly and efficiently test the entire process with all of its possibilities.

In the `Investment Application` sample process, there are a total of four different scenarios:
- Manager's refusal
- Manager's approval of an amount less than $15K
- Finance Comptroller's refusal of an amount greater than $15K
- Finance Comptroller's approval of an amount greater than $15K

You can test these scenarios with the `tests.json` file.

# Implementation

- Import the `INVESTMENT_APP_TEMPLATEv1.xml` process on your instance
- Execute the command `wfg process test tests.json`'