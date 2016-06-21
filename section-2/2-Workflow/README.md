#Workflow

## Starting a new project

When you start working on a new project, follow the README.md file located in the root directory of the project to set up.

Our projects are in two separate projects in most cases. One is for the backend (or API), the other is for the frontend (or client) code.

The project contain a bash script `.danger` that can should be run when you first set up the project:
```bash
./.danger
```

It will provide you with information about missing dependencies, and so on. Pay attention to the output, and follow recommendations.

If you need help, contact one of us on #slack. 

## Sprints

We organize our workload into Sprints. (Read more about the [Agile development process](https://en.wikipedia.org/wiki/Agile_software_development) on Wikipedia.)


## Working on a task (Git workflow)

Our general workflow is based on the [GitHub Flow](https://guides.github.com/introduction/flow/). (The major difference is that we "deploy" to the development branch for further QA from the client.)

Here is the detailed workflow:

1. You get assigned to a task (an issue on the github repo, appears as a ticket on the waffle board)
1. As soon as you start reading the specifications, move your task into the "In progress" column on waffle.
1. Create a new branch: `git checkout -b <issue-number>-<kebab-cased-issue-name>`
1. Once a sub-task (an item with a check-box in the issue description) is complete, commit your work, and push. Once pushed, tick the corresponding check-box.
1. Refactor your code, commit, and push.
1. Repeat steps 4 and 5 until your task is done.
1. Create a new Pull Request on your branch.
1. Move your ticket to 'For testing' on waffle'.
1. Let a gluer know your task is ready for reviewing.
1. A gluer will review your task, and move it back to 'In progress' if further work is needed.
1. Repeat steps 4-10 until task marked done by the gluer.

> **Note** If you are stuck with anything for over 15 minutes, let one of the gluers know on slack.

### Communication
Our communication protocol is aimed at providing the most efficient help for everyone, while not distracting people too much from their own workflow.

Please follow these steps when you are facing a problem.

1. If you face a problem, try to solve it. Most issues are easily resolved by google + stack overflow.
1. If you haven't got a solution in 15 minutes, ask your question in the #students room. Try to answer others' questions in this room.
1. If after a further 10 minutes there is no answer (or no one is online in the #students room), ask your question in the project room
1. If no one responds, try direct messaging someone.

> **Note** **The last thing we want is to discourage you from asking questions**. However the gluers need to get on with our own development tasks, too.

### Commit message syntax
Your commit messages should be in the imperative form (as if you are telling git what to do) and be capitalised.

The commit message must start with the issue number you are working on.

Correct examples:  
`#213: Add new API endpoint to retrieve Users`  
`#214: Fix broken tests for Users`  
`#215: Hook up User Profile frontend with API`  

Incorrect examples:  
`added new API endpoint to retrieve Users` (no issue number, no capital, not imperative)  
`#215: add new API endpoint to retrieve Users` (no capital A)  
`#214: Added new API endpoint to retrieve Users` (not imperative form)  
