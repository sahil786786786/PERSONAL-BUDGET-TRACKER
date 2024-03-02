# PERSONAL-BUDGET-TRACKER
import json
import os
from datetime import datetime

# File to store tasks
TASKS_FILE = "tasks.json"

def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, 'r') as file:
            return json.load(file)
    else:
        return []

def save_tasks(tasks):
    with open(TASKS_FILE, 'w') as file:
        json.dump(tasks, file, indent=4)

def add_task(tasks, title, priority, due_date):
    task = {"title": title, "priority": priority, "due_date": due_date, "completed": False}
    tasks.append(task)
    save_tasks(tasks)

def remove_task(tasks, index):
    if 0 <= index < len(tasks):
        del tasks[index]
        save_tasks(tasks)
    else:
        print("Invalid task index.")

def mark_task_completed(tasks, index):
    if 0 <= index < len(tasks):
        tasks[index]["completed"] = True
        save_tasks(tasks)
    else:
        print("Invalid task index.")

def list_tasks(tasks):
    print("Task List:")
    for index, task in enumerate(tasks):
        status = "[X]" if task["completed"] else "[ ]"
        print(f"{index + 1}. {status} {task['title']} - Priority: {task['priority']}, Due Date: {task['due_date']}")

def main():
    tasks = load_tasks()
    
    while True:
        print("\nTo-Do List")
        print("1. Add Task")
        print("2. Remove Task")
        print("3. Mark Task as Completed")
        print("4. List Tasks")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            title = input("Enter task title: ")
            priority = input("Enter task priority (high/medium/low): ")
            due_date = input("Enter due date (YYYY-MM-DD): ")
            add_task(tasks, title, priority, due_date)
            print("Task added successfully!")

        elif choice == "2":
            list_tasks(tasks)
            index = int(input("Enter task index to remove: ")) - 1
            remove_task(tasks, index)

        elif choice == "3":
            list_tasks(tasks)
            index = int(input("Enter task index to mark as completed: ")) - 1
            mark_task_completed(tasks, index)

        elif choice == "4":
            list_tasks(tasks)

        elif choice == "5":
            print("Exiting...")
            break

        else:
            print("Invalid choice. Please choose again.")

if __name__ == "__main__":
    main()
