The challenge is to create a set of algorithms for picking tasks from a todo list and moving them to a processing list according to the algorithm’s rules. Each task should only exist in the processing list for an amount of time specified in the customer object that the task is associated with. Once a task has been processed (the time has elapsed), it should be moved back to the todo list again, to be picked up once more (keeping the system in a continuous flow). Depending on the algorithm, there should be a varying number of tasks in processing at any given time.

# Additional details:
1.	task objects have the following structure:
{
    _id: "<UUID>",
    customerId: "<CUSTOMER _id>",
    insertedTime: "2018-03-14T15:48:14+0000"
}

2.	customer objects have the following structure:
{
    _id: "<UUID>",
    taskMinSeconds: 5,
    taskMaxSeconds: 15,
}
3.	All tasks are associated with a customer.
4.	The todo and processing lists contain tasks.
5.	taskMinSeconds and taskMaxSeconds determine how long a task is supposed to exist in the processing list before it is moved back to the todo list. The specific amount of time for a specific task should be determined just before the task is placed into the processing list and should be equal to a random number of seconds in between taskMinSeconds and taskMaxSeconds.
6.	Once a task is chosen for processing by the algorithm, it should be removed from the todo list and added to the processing list.
7.	Once the task has existed in the processing list for an amount of time equal to that task’s processing time (based on above), it should be removed from the processing list and re-inserted into the todo list with a new insertedTime. Once the task is removed from processing, another task should be chosen by the algorithm to be placed into the processing list.
8.	The processing list will be limited in total size by the ‘MAX_TASKS_PROCESSING’ environmental variable. The number of tasks in the processing list may not exceed this value. The default value for this should be 100.
Across the algorithms:
1.	Ideally, the customers, todo and processing lists should be implemented as database collections or tables. You may use a database of your choice to do this. They may be implemented in memory as long as there is a convenient way to observe the contents of the lists during program execution.
2.	You may add properties to the customer and task objects as you see fit.
3.	The taskMinSeconds and taskMaxSeconds properties of the customer object will need to be adjustable while the application is running. We will need to be able to see the count of tasks in processing reflect these changes (based on the algorithm).
4.	During code review, while the application is running, you will need to display various information about the tasks in the todo and processing lists (such as the count of tasks grouped by customer).
5.	You will need to implement the following algorithms and display them being applied as described.
6.	You will need a way to quickly reset the system to display the use of the algorithms in isolation.

# Algorithms
### First in first out (FIFO)
For this algorithm, tasks should be selected from the todo list simply based on their insertedTime. The customer the tasks are associated with is not pertinent to the algorithm. The task chosen to be placed in the processing list should be the task with the earliest insertedTime of all todo tasks.
### Round Robin
For this algorithm, tasks should be selected from the todo list based on the customer they are associated with. This should be done in a round robin fashion based on the customers. For example, if there are 3 customers, the first task is chosen for customer1, then a task for customer2, customer3, then back to customer1, and so on. When choosing a task narrowed down to a specific customer (e.g knowing that you are choosing a task for customer1), the task with the earliest insertedTime should be selected.
### Balanced Round Robin
For this algorithm, tasks should be selected from the todo list based on the customer they are associated with. This should be done in a round robin fashion based on the customers and the number of tasks currently in the processing list. The goal of this algorithm is to have the same number of tasks for each customer in the processing list at any time. This should be demonstrated by setting much higher values for  taskMinSeconds and taskMaxSeconds for a specific customer, and showing that tasks are still distributed evenly across all customers. One customer should not dominate the processing list.

## Setup
#### Requirements
- Install node (https://nodejs.org)
#### Installation
- Run `npm install` from the root directory

## Running the program
To run the program with each algorithm use the following commands:

#### First In First Out
- `npm run fifo`

#### Round Robin
- `npm run round`

#### Balanced Round Robin
- `npm run balanced`

The task processor will run continuously, it can be stopped at any time by pressing any key and restarted with the above commands.

To run the tests:

- `npm run test`
