using System;
using System.Collections.Generic;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;
using System.Linq;

[Serializable]
public class Task
{
    public string Title { get; set; }
    public DateTime DueDate { get; set; }
    public string Status { get; set; }
    public string Project { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        List<Task> tasks = LoadTasks();

        while (true)
        {
            Console.WriteLine("Welcome to ToDoLy");
            Console.WriteLine($"You have {tasks.Count(t => t.Status != "Done")} tasks to do and {tasks.Count(t => t.Status == "Done")} tasks are done!");
            Console.WriteLine("Pick an option:");
            Console.WriteLine("(1) Show Task List (by date or project)");
            Console.WriteLine("(2) Add New Task");
            Console.WriteLine("(3) Edit Task (Update, mark as done, remove)");
            Console.WriteLine("(4) Save and Quit");

            string option = Console.ReadLine();

            switch (option)
            {
                case "1":
                    ShowTasks(tasks);
                    break;
                case "2":
                    AddNewTask(tasks);
                    break;
                case "3":
                    EditTask(tasks);
                    break;
                case "4":
                    SaveTasks(tasks);
                    return;
                default:
                    Console.WriteLine("Invalid option. Try again.");
                    break;
            }
        }
    }

    static void ShowTasks(List<Task> tasks)
    {
        Console.WriteLine("Sort by: (1) Date (2) Project");
        string sortOption = Console.ReadLine();
        if (sortOption == "1")
        {
            tasks = tasks.OrderBy(t => t.DueDate).ToList();
        }
        else if (sortOption == "2")
        {
            tasks = tasks.OrderBy(t => t.Project).ToList();
        }

        Console.WriteLine("Tasks:");
        for (int i = 0; i < tasks.Count; i++)
        {
            Console.WriteLine($"{i + 1}. {tasks[i].Title} | {tasks[i].DueDate} | {tasks[i].Status} | {tasks[i].Project}");
        }
    }

    static void AddNewTask(List<Task> tasks)
    {
        Console.WriteLine("Enter task title:");
        string title = Console.ReadLine();
        Console.WriteLine("Enter due date (yyyy-mm-dd):");
        DateTime dueDate = DateTime.Parse(Console.ReadLine());
        Console.WriteLine("Enter project:");
        string project = Console.ReadLine();

        Task newTask = new Task
        {
            Title = title,
            DueDate = dueDate,
            Status = "Pending",
            Project = project
        };

        tasks.Add(newTask);
    }

    static void EditTask(List<Task> tasks)
    {
        ShowTasks(tasks);
        Console.WriteLine("Enter task number to edit:");
        int taskNumber = int.Parse(Console.ReadLine()) - 1;

        if (taskNumber >= 0 && taskNumber < tasks.Count)
        {
            Console.WriteLine("Choose an option: (1) Update (2) Mark as done (3) Remove");
            string editOption = Console.ReadLine();
            if (editOption == "1")
            {
                Console.WriteLine("Enter new title:");
                tasks[taskNumber].Title = Console.ReadLine();
                Console.WriteLine("Enter new due date (yyyy-mm-dd):");
                tasks[taskNumber].DueDate = DateTime.Parse(Console.ReadLine());
                Console.WriteLine("Enter new project:");
                tasks[taskNumber].Project = Console.ReadLine();
            }
            else if (editOption == "2")
            {
                tasks[taskNumber].Status = "Done";
            }
            else if (editOption == "3")
            {
                tasks.RemoveAt(taskNumber);
            }
        }
        else
        {
            Console.WriteLine("Invalid task number.");
        }
    }

    static void SaveTasks(List<Task> tasks)
    {
        using (FileStream fs = new FileStream("tasks.dat", FileMode.Create))
        {
            BinaryFormatter formatter = new BinaryFormatter();
            formatter.Serialize(fs, tasks);
        }
    }

    static List<Task> LoadTasks()
    {
        if (File.Exists("tasks.dat"))
        {
            using (FileStream fs = new FileStream("tasks.dat", FileMode.Open))
            {
                BinaryFormatter formatter = new BinaryFormatter();
                return (List<Task>)formatter.Deserialize(fs);
            }
        }
        return new List<Task>();
    }
}
