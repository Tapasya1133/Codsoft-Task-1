# Codsoft-Task-1
import tkinter as tk
from tkinter import messagebox


class ToDoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To-Do List with Categories")
        self.tasks = []




        input_frame = tk.Frame(root)
        input_frame.pack(pady=10)

        self.task_entry = tk.Entry(input_frame, width=30)
        self.task_entry.grid(row=0, column=0, padx=5)

        self.category_entry = tk.Entry(input_frame, width=15)
        self.category_entry.grid(row=0, column=1, padx=5)
        self.category_entry.insert(0, "Category")

        add_button = tk.Button(input_frame, text="Add Task", command=self.add_task)
        add_button.grid(row=0, column=2, padx=5)


        filter_frame = tk.Frame(root)
        filter_frame.pack(pady=5)

        self.filter_var = tk.StringVar()
        self.filter_entry = tk.Entry(filter_frame, textvariable=self.filter_var, width=20)
        self.filter_entry.grid(row=0, column=0, padx=5)

        filter_button = tk.Button(filter_frame, text="Filter", command=self.filter_tasks)
        filter_button.grid(row=0, column=1)

        show_all_button = tk.Button(filter_frame, text="Show All", command=self.show_all_tasks)
        show_all_button.grid(row=0, column=2)


        self.listbox = tk.Listbox(root, width=50, height=10)
        self.listbox.pack(pady=10)


        btn_frame = tk.Frame(root)
        btn_frame.pack()

        delete_btn = tk.Button(btn_frame, text="Delete Task", command=self.delete_task)
        delete_btn.grid(row=0, column=0, padx=10)

        done_btn = tk.Button(btn_frame, text="Mark as Done", command=self.mark_done)
        done_btn.grid(row=0, column=1)

    def add_task(self):
        task = self.task_entry.get().strip()
        category = self.category_entry.get().strip()

        if task == "":
            messagebox.showwarning("Input Error", "Task cannot be empty.")
            return

        full_task = f"{task} [{category}]"
        self.tasks.append(full_task)
        self.update_listbox()

        self.task_entry.delete(0, tk.END)
        self.category_entry.delete(0, tk.END)

    def delete_task(self):
        selected = self.listbox.curselection()
        if not selected:
            messagebox.showwarning("Select Task", "Please select a task to delete.")
            return

        task_text = self.listbox.get(selected[0])
        self.tasks.remove(task_text)
        self.update_listbox()

    def mark_done(self):
        selected = self.listbox.curselection()
        if not selected:
            messagebox.showwarning("Select Task", "Please select a task to mark as done.")
            return

        task_text = self.listbox.get(selected[0])
        if not task_text.startswith("[Done]"):
            new_task = "[Done] " + task_text
            self.tasks[selected[0]] = new_task
            self.update_listbox()

    def filter_tasks(self):
        keyword = self.filter_var.get().strip().lower()
        filtered = [task for task in self.tasks if keyword in task.lower()]
        self.update_listbox(filtered)

    def show_all_tasks(self):
        self.update_listbox()

    def update_listbox(self, tasks=None):
        self.listbox.delete(0, tk.END)
        tasks_to_show = tasks if tasks is not None else self.tasks
        for task in tasks_to_show:
            self.listbox.insert(tk.END, task)

if __name__ == "__main__":
    root = tk.Tk()
    app = ToDoApp(root)
    root.mainloop()
