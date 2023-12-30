An MMORPG game is under development. For the profile and inventory mechanics, it needs a query that calculates a list of game accounts whose inventory is overloaded with game items.The result should have the following columns: username | email | items | total_weight
username - account username
email - account email address
items - total number of items in inventory
total_weight  - total weight of items in inventory
The result should be sorted in descending order by total_weight, then in ascending order by username

Note:
Each item in the inventory has its own weight.
Only accounts where the total weight of all items in the inventory exceeds the overload threshold should be included in the result.
The overload threshold is 20.


    SELECT
    a.username,
    a.email,
    COUNT(ai.item_id) AS items,
    SUM(i.weight) AS total_weight
    FROM
    accounts AS a
    INNER JOIN
    accounts_items AS ai ON a.id = ai.account_id
    INNER JOIN
    items AS i ON ai.item_id = i.id
    GROUP BY
    a.username, a.email
    HAVING
    SUM(i.weight) > 20
    ORDER BY
    total_weight DESC, username ASC;


When multiple tasks are executed on a single- threaded CPU, the tasks are scheduled based on the principle of pre-emption. When a higher- priority task arrives in the execution queue, then the lower-priority task is pre-empted, i.e. its execution is paused until the higher-priority task is complete. There are n functions to be executed on a single- threaded CPU, with each function having a unique ID between 0 and n - 1. Given an integer n, representing the number of functions to be executed, and an execution log as an array of strings, logs, of size m, determine the exclusive times of each of the functions. Exclusive time is the sum of execution times for all calls to a function. Any string representing an execution log is of the form {function_id}{"start" "end"}: {timestamp), indicating that the function with ID function_id, either starts or ends at a time identified by the timestamp value. 4 = Note: While calculating the execution time of a function call, both the starting and ending times of the function call have to be included. The log of the form (function_id}:{start}: {timestamp)} means that the running function is preempted at the beginning of timestamp second. The log of the form (function_id}: {end}: {timestamp} means that the function function_id is preempted after completing its execution at timestamp second i.e after timestamp second. Example​


#!/bin/python3

import math
import os
import random
import re
import sys

def getTotalExecutionTime(n, logs):
    result = [0] * n
    stack = []
    prev_timestamp = 0

    for log in logs:
        log_parts = log.split(':')
        function_id, action, timestamp = int(log_parts[0]), log_parts[1], int(log_parts[2])

        if action == "start":
            if stack:
                result[stack[-1]] += timestamp - prev_timestamp
            stack.append(function_id)
            prev_timestamp = timestamp
        else:  # action == "end"
            result[stack.pop()] += timestamp - prev_timestamp + 1
            prev_timestamp = timestamp + 1

    return result

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    n = int(input().strip())
    logs_count = int(input().strip())
    logs = []

    for _ in range(logs_count):
        logs_item = input()
        logs.append(logs_item)

    result = getTotalExecutionTime(n, logs)

    fptr.write('\n'.join(map(str, result)))
    fptr.write('\n')

    fptr.close()
