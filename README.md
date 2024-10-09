import tkinter as tk
from tkinter import messagebox, simpledialog

class TodoApp:
    def __init__(self, master):
        self.master = master
        self.master.title("To-Do List")

        self.tasks = []  # List to store tasks and their completion status
        
        # Create GUI components
        self.task_frame = tk.Frame(self.master)
        self.task_frame.pack(pady=10)

        self.task_label = tk.Label(self.task_frame, text="Enter a task:")
        self.task_label.pack(side=tk.LEFT)

        self.task_entry = tk.Entry(self.task_frame, width=30)
        self.task_entry.pack(side=tk.LEFT)

        self.add_task_button = tk.Button(self.task_frame, text="Add Task", command=self.add_task)
        self.add_task_button.pack(side=tk.LEFT)

        self.tasks_listbox = tk.Listbox(self.master, selectmode=tk.SINGLE, width=50, height=10)
        self.tasks_listbox.pack(pady=10)

        self.complete_button = tk.Button(self.master, text="Mark as Complete", command=self.mark_complete)
        self.complete_button.pack(pady=5)

        self.remove_task_button = tk.Button(self.master, text="Remove Task", command=self.remove_task)
        self.remove_task_button.pack(pady=5)

        self.update_button = tk.Button(self.master, text="Update List", command=self.update_tasks_listbox)
        self.update_button.pack(pady=5)

    def add_task(self):
        task = self.task_entry.get()
        if task:
            self.tasks.append((task, False))  # Append tuple (task, completed status)
            self.update_tasks_listbox()
            self.task_entry.delete(0, tk.END)  # Clear the entry
        else:
            messagebox.showwarning("Warning", "Please enter a task.")

    def mark_complete(self):
        try:
            selected_task_index = self.tasks_listbox.curselection()[0]
            task, completed = self.tasks[selected_task_index]
            self.tasks[selected_task_index] = (task, not completed)  # Toggle completion status
            self.update_tasks_listbox()
        except IndexError:
            messagebox.showwarning("Warning", "Please select a task to mark as complete.")

    def remove_task(self):
        try:
            selected_task_index = self.tasks_listbox.curselection()[0]
            self.tasks.pop(selected_task_index)
            self.update_tasks_listbox()
        except IndexError:
            messagebox.showwarning("Warning", "Please select a task to remove.")

    def update_tasks_listbox(self):
        self.tasks_listbox.delete(0, tk.END)  # Clear the current list
        for task, completed in self.tasks:
            status = "✓" if completed else "✗"  # Use checkmark for completed, cross for not completed
            self.tasks_listbox.insert(tk.END, f"{status} {task}")  # Insert updated tasks

def main():
    root = tk.Tk()
    app = TodoApp(root)
    root.mainloop()

if __name__ == "__main__":
    main()
