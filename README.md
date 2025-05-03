# observer


**Описание программы:**

Программа имитирует ситуацию в университете, где преподаватель заканчивает занятие и говорит студентам, что они могут идти домой. Студенты реагируют на это событие по-разному. Мы используем паттерн Observer, чтобы преподаватель (Subject) не зависел от конкретных действий студентов (Observers).

**Язык:** C#

**Классы:**

*   **`Teacher` (Subject):** Класс преподавателя. Имеет метод `WrapUp()`, который уведомляет всех студентов о завершении занятия.
*   **`Student` (Observer):** Абстрактный класс студента. Имеет метод `ReactToWrapUp()`, который определяет реакцию студента на завершение занятия.
*   **`ConcreteStudent1`, `ConcreteStudent2`, ... (Concrete Observers):** Конкретные классы студентов, которые реализуют метод `ReactToWrapUp()` по-своему.

**Observer Pattern:**

*   **Subject (Observable):** Класс, за состоянием которого наблюдают. В нашем случае это `Teacher`.
*   **Observer:** Интерфейс или абстрактный класс, определяющий метод для реакции на изменения состояния Subject. В нашем случае это `Student`.
*   **ConcreteObserver:** Конкретные классы, реализующие интерфейс Observer и определяющие собственное поведение. В нашем случае это `ConcreteStudent1`, `ConcreteStudent2` и т.д.

**Код (C#):**

```csharp
using System;
using System.Collections.Generic;

// Observer interface
public abstract class Student
{
    public string Name { get; set; }
    public Student(string name)
    {
        Name = name;
    }
    public abstract void ReactToWrapUp();
}

// Concrete Observers
public class DiligentStudent : Student
{
    public DiligentStudent(string name) : base(name) { }
    public override void ReactToWrapUp()
    {
        Console.WriteLine($"{Name}: Starts packing his/her stuff carefully.");
    }
}

public class TechSavvyStudent : Student
{
    public TechSavvyStudent(string name) : base(name) { }
    public override void ReactToWrapUp()
    {
        Console.WriteLine($"{Name}: Shuts down the computer quickly.");
    }
}

public class SocialStudent : Student
{
    public SocialStudent(string name) : base(name) { }
    public override void ReactToWrapUp()
    {
        Console.WriteLine($"{Name}: Starts chatting with classmates about weekend plans.");
    }
}

public class SleepyStudent : Student
{
    public SleepyStudent(string name) : base(name) { }
    public override void ReactToWrapUp()
    {
        Console.WriteLine($"{Name}: Wakes up with a jolt and wonders if class is really over.");
    }
}

// Subject
public class Teacher
{
    private List<Student> students = new List<Student>();

    public void AddStudent(Student student)
    {
        students.Add(student);
    }

    public void RemoveStudent(Student student)
    {
        students.Remove(student);
    }

    public void WrapUp()
    {
        Console.WriteLine("Teacher: Okay, class, you can pack up and go home!");
        NotifyStudents();
    }

    private void NotifyStudents()
    {
        foreach (var student in students)
        {
            student.ReactToWrapUp();
        }
    }
}

// Main Program
public class Program
{
    public static void Main(string[] args)
    {
        // Create teacher
        Teacher teacher = new Teacher();

        // Create students
        Student john = new DiligentStudent("John");
        Student alice = new TechSavvyStudent("Alice");
        Student bob = new SocialStudent("Bob");
        Student emily = new SleepyStudent("Emily");

        // Register students with the teacher
        teacher.AddStudent(john);
        teacher.AddStudent(alice);
        teacher.AddStudent(bob);
        teacher.AddStudent(emily);

        // Teacher finishes the class
        teacher.WrapUp();

        Console.ReadKey();
    }
}
```

**Объяснение кода:**

1.  **`Student` (Observer):**
    *   Абстрактный класс, определяющий метод `ReactToWrapUp()`, который должны реализовать все конкретные студенты.
    *   Имеет свойство `Name` для хранения имени студента.

2.  **`DiligentStudent`, `TechSavvyStudent`, `SocialStudent`, `SleepyStudent` (Concrete Observers):**
    *   Конкретные классы студентов, которые наследуются от `Student` и реализуют метод `ReactToWrapUp()` по-своему.  Каждый студент имеет свою уникальную реакцию на завершение занятия.

3.  **`Teacher` (Subject):**
    *   Содержит список зарегистрированных студентов (`students`).
    *   Метод `AddStudent()` позволяет добавлять студентов в список.
    *   Метод `RemoveStudent()` позволяет удалять студентов из списка.
    *   Метод `WrapUp()` вызывает метод `NotifyStudents()`, который уведомляет всех зарегистрированных студентов о завершении занятия, вызывая их метод `ReactToWrapUp()`.

4.  **`Program` (Main):**
    *   Создает экземпляры классов `Teacher` и `Student`.
    *   Регистрирует студентов у преподавателя с помощью метода `AddStudent()`.
    *   Вызывает метод `WrapUp()` у преподавателя, чтобы имитировать завершение занятия.

**Как запустить:**

1.  Сохраните код в файл с расширением `.cs` (например, `Program.cs`).
2.  Убедитесь, что у вас установлен .NET SDK.
3.  Откройте командную строку или терминал.
4.  Перейдите в каталог, где находится файл `Program.cs`.
5.  Выполните команду `dotnet run`.

**Вывод программы:**

```
Teacher: Okay, class, you can pack up and go home!
John: Starts packing his/her stuff carefully.
Alice: Shuts down the computer quickly.
Bob: Starts chatting with classmates about weekend plans.
Emily: Wakes up with a jolt and wonders if class is really over.
```

**Преимущества использования Observer Pattern:**

*   **Слабая связанность:** Преподаватель не знает конкретных действий студентов. Он просто уведомляет их о событии. Это позволяет добавлять новых студентов и изменять поведение существующих без изменения класса `Teacher`.
*   **Гибкость:** Каждый студент может реагировать на завершение занятия по-своему.
*   **Масштабируемость:** Легко добавлять новых студентов и изменять их поведение.

