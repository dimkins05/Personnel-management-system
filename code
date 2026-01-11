using EmployeeManagementSystem.Core.Interfaces;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace EmployeeManagementSystem.Core.Models
{
    /// <summary>
    /// Класс компании
    /// </summary>
    [Serializable]
    public class Company : IReportable
    {
        #region Свойства
        // Свойства
        public string Name { get; set; }
        public List<Department> Departments { get; private set; }
        public Employee CEO { get; set; }
        public List<Project> Projects { get; private set; }
        #endregion
        
        #region Конструктор
        // Конструктор
        /// <summary>
        /// Конструктор
        /// </summary>
        /// <param name="name"></param>
        /// <param name="ceo"></param>
        public Company(string name, Employee ceo)
        {
            Name = name;
            CEO = ceo;
            Departments = new List<Department>();
            Projects = new List<Project>();
        }
        #endregion
        
        #region Методы для управления отделами
        // Методы для управления отделами
        /// <summary>
        /// Добавление отдела
        /// </summary>
        /// <param name="department"></param>
        public void AddDepartment(Department department)
        {
            Departments.Add(department);
        }

        /// <summary>
        /// Удаление отдела
        /// </summary>
        /// <param name="department"></param>
        /// <returns></returns>
        public bool RemoveDepartment(Department department)
        {
            // нельзя удалить отдел в котором есть сотрудники
            if (department.Employees.Count > 0)
            {
                return false;
            }
            return Departments.Remove(department);
        }

        #endregion

        #region Методы для работы с проектами
        /// Методы для работы с проектами

        /// <summary>
        /// Добавление проекта
        /// </summary>
        /// <param name="project"></param>
        public void AddProject(Project project)
        {
            Projects.Add(project);
        }

        /// <summary>
        /// Удаление проекта
        /// </summary>
        /// <param name="projectId"></param>
        /// <returns></returns>
        public bool RemoveProject(int projectId)
        {
            for (int i = 0; i < Projects.Count; i++)
            {
                if (Projects[i].Id == projectId)
                {
                    Projects.RemoveAt(i);
                    return true;
                }
            }
            return false;
        }

        public bool RemoveProject(Project project)
        {
            // TODO RemoveProject Assignments
            return Projects.Remove(project);
        }

        public string GetProjectsList(bool showOrderNumber = false)
        {
            StringBuilder sb = new StringBuilder();

            string header = showOrderNumber
                ? $"\n{"№",-5}{"Название",-40}{"ID",-7}{"Статус",-14}{"Бюджет",16}{"Членов команды",-18}{"Менеджер",-24}"
                : $"\n{"Название",-40}{"ID",-7}{"Статус",-14}{"Бюджет",-16}{"Членов команды",-18}{"Менеджер",-24}";

            sb.AppendLine(header);
            sb.AppendLine(new string('-', showOrderNumber ? 117 : 112));

            //string header = showOrderNumber
            //    ? $"\n{"№",-5}{"Название",-40}{"ID",-7}{"Статус",-20}{"Бюджет",-20}{"Членов команды",-20}{"ПР",+ new string('-', showOrderNumber ? 107 : 102) + "\n"
            //    : $"\n{"Название",-40}{"ID",-7}{"Статус",-20}{"Бюджет",-20}{"Членов команды",-20}{"ПР",+ new string('-', 102) + "\n";

            for (int i = 0; i < Projects.Count; i++)
            {
                string projectsReport = $"{Projects[i].Name,-40}{Projects[i].Id,-7}{Projects[i].Status,-14}{Projects[i].Budget,-20:c2}{Projects[i].Team.Count,-14}{Projects[i].ProjectManager?.Name,-24}";

                if (showOrderNumber)
                {
                    projectsReport = $"{i + 1,-5}" + projectsReport;
                }
                sb.AppendLine(projectsReport);
            }
            return sb.ToString();
        }

        #endregion

        #region Методы для работы с сотрудниками
        /// <summary>
        /// Получение общего количества сотрудников
        /// </summary>
        /// <returns>Число сотрудников</returns>
        public int GetTotalEmployees()
        {
            int totalEmployees = 0;

            for (int i = 0; i < Departments.Count; i++)
            {
                totalEmployees += Departments[i].Employees.Count;
            }

            // Учитываем CEO отдельно, так как он может не принадлежать ни к одному департаменту
            // Добавляем CEO, если он не включен в какой-либо отдел

            if (CEO != null && CEO.Department == null)
            //if (CEO != null && CEO.DepartmentId == 0)
            {
                totalEmployees++;
            }

            return totalEmployees;
        }

        private List<Employee> GetAllEmployeesList()
        {
            List<Employee> allEmployees = new List<Employee>();
            // Добавляем CEO, если он существует (он может быть не сотрудником компании)

            if (CEO != null)
            {
                allEmployees.Add(CEO);
            }

            // Добавляем сотрудников из всех отделов
            for (int i = 0; i < Departments.Count; i++)
            {
                for (int j = 0; j < Departments[i].Employees.Count; j++)
                {
                    // Проверяем, что сотрудник не является CEO
                    if (Departments[i].Employees[j].Id != CEO?.Id)
                    {
                        allEmployees.Add(Departments[i].Employees[j]);
                    }
                }
            }

            return allEmployees;
        }

        /// <summary>
        /// Получение сотрудника по имени
        /// </summary>
        /// <param name="name"></param>
        /// <returns>Сотрудник</returns>
        public Employee GetEmployeeByName(string name)
        {
            var employees = GetAllEmployeesList();
            foreach (var employee in employees)
            {
                if (employee.Name.ToLower() == name.ToLower())
                {
                    return employee;
                }
            }
            return null;
        }

        public IEnumerable<Employee> GetAllEmployees()
        {
            return GetAllEmployeesList();
        }

        #endregion
        
        #region Реализация интерфейса IReportable
        // Реализация интерфейса IReportable

        /// <summary>
        /// Формирование отчета
        /// </summary>
        /// <param name="format"></param>
        /// <returns></returns>
        public string GenerateReport(string format)
        {
            if (format.ToLower() == "short")
            {
                return $"Компания: {Name}, Отделов: {Departments.Count}, Проектов: {Projects.Count}, Сотрудников: {GetTotalEmployees()}";
            }

            string departmentsReport = "Отделы:\n";
            for (int i = 0; i < Departments.Count; i++)
            {
                departmentsReport += $" - {Departments[i].Name} (ID: {Departments[i].Id})\n";
            }
            string projectsReport = "Проекты:\n";
            for (int i = 0; i < Projects.Count; i++)
            {
                projectsReport += $" - {Projects[i].Name} (ID: {Projects[i].Id}, Status: {Projects[i].Status})\n";
            }
            return $"Компания: {Name}\nCEO: {(CEO != null ? CEO.Name : "Не назначен")}\n"
                + $"Всего сотрудников: {GetTotalEmployees()}\n"
                + $"Отделов: {Departments.Count}\n{departmentsReport}\n"
                + $"Проектов: {Projects.Count}\n{projectsReport}";
        }

        #endregion
        
        #region Получение списка отделов
        /// <summary>
        /// Получение списка отделов
        /// </summary>
        /// <param name="showOrderNumber"></param>
        /// <returns>Строка содержащая список отделов компании</returns>
        public string GetDepartmentsList(bool showOrderNumber = false)
        {
            StringBuilder sb = new StringBuilder();

            sb.AppendLine("СПИСОК ОТДЕЛОВ:\n---\n");

            string header = showOrderNumber
                ? $"{"№",-3}{"ID",-7}{"Название",-30}{"Сотрудников",-22}{"Руководитель",-30}{"Бюджет",-15}{"Остаток бюджета",-16}"
                : $"{"ID",-7}{"Название",-30}{"Сотрудников",-22}{"Руководитель",30}{"Бюджет",-15}{"Остаток бюджета",-16}";

            sb.AppendLine(header);
            sb.AppendLine(new string('-', showOrderNumber ? 123 : 120));

            for (int i = 0; i < Departments.Count; i++)
            {
                string departmentInfo = Departments[i].GenerateReport("short");
                if (showOrderNumber)
                {
                    departmentInfo = $"{i + 1,-3}" + departmentInfo;
                }
                sb.AppendLine(departmentInfo);
            }

            return sb.ToString();
        }

        #endregion

        #region Получение списка всех сотрудников
        /// <summary>
        /// Получение списка всех сотрудников
        /// </summary>
        /// <param name="showOrderNumber"></param>
        /// <returns>Строка содержащая список всех сотрудников компании</returns>
        public string GetAllEmployeeListString(bool showOrderNumber = false)
        {
            StringBuilder sb = new StringBuilder();

            //sb.AppendLine("СПИСОК ВСЕХ СОТРУДНИКОВ\n");

            string header = showOrderNumber
                ? $"{"№",-5}{"ID",-5}{"Имя",-30}{"Тип сотрудника",-20}{"Отдел",-30}{"Стаж",-10}{"Ставка",-10}"
                : $"{"ID",-5}{"Имя",-30}{"Тип сотрудника",-20}{"Отдел",-30}{"Стаж",10}{"Ставка",-10}";

            sb.AppendLine(header);
            sb.AppendLine(new string('-', showOrderNumber ? 110 : 105));

            var employees = GetAllEmployeesList();
            for (int i = 0; i < employees.Count; i++)
            {
                var employee = employees[i];
                string departmentName = employee.Department?.Name ?? "Нет отдела";
                string employeeInfo = $"{employee.Id,-5}{employee.Name,-30}{employee.GetType().Name,-20}{departmentName,-30}{employee.CalculateExperienceString(),-10}{employee.CalculateSalary(),-10:C2}";

                if (showOrderNumber)
                {
                    employeeInfo = $"{i + 1,-5}" + employeeInfo;
                }

                sb.AppendLine(employeeInfo);
            }
            return sb.ToString();
        }
        #endregion
    }
}

using EmployeeManagementSystem.Core.Interfaces;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace EmployeeManagementSystem.Core.Models
{
    /// <summary>
    /// Класс отдел
    /// </summary>
    [Serializable]
    public class Department : IReportable
    {
        #region Свойства
        // Свойства
        public int Id { get; set; }
        public string Name { get; set; }
        public Manager Head { get; private set; }
        public List<Employee> Employees { get; private set; }
        public decimal Budget { get; set; }

        #endregion

        #region Конструктор
        // Конструктор

        /// <summary>
        /// Конструктор
        /// </summary>
        /// <param name="id"></param>
        /// <param name="name"></param>
        /// <param name="head"></param>
        /// <param name="budget"></param>
        public Department(int id, string name, Manager head, decimal budget)
        {
            Id = id;
            Name = name;
            Head = head;
            Budget = budget;
            Employees = new List<Employee>();
            if (head != null)
            {
                SetHead(head);
                //Employees.Add(head);
            }
        }

        #endregion

        #region Метод для назначения главы отдела
        // Метод для назначения главы отдела

        /// <summary>
        /// Метод для назначения главы отдела
        /// </summary>
        /// <param name="newHead"></param>
        /// <exception cref="ArgumentNullException"></exception>
        public void SetHead(Manager newHead)
        {
            if (newHead == null)
                throw new ArgumentNullException(nameof(newHead));

            // Удаляем текущего главу из списка сотрудников (если он есть)
            if (Head != null)
            {
                Employees.Remove(Head);
            }

            // Назначаем нового главу
            Head = newHead;
            if (!Employees.Contains(newHead))
            {
                Employees.Add(newHead);
                newHead.Department = this; // ссылка на этот отдел
            }

            // Все сотрудники отдела становятся подчинёнными нового главы
            foreach (var employee in Employees)
            {
                if (employee.Id != newHead.Id) // Проверяем, чтобы глава не был подчинённым самому себе
                {
                    newHead.AddSubordinate(employee);
                }
            }
        }

        #endregion

        #region Методы для управления сотрудниками
        // Методы для управления сотрудниками

        /// <summary>
        /// Добавление сотрудника
        /// </summary>
        /// <param name="employee"></param>
        /// <exception cref="ArgumentNullException"></exception>
        public void AddEmployee(Employee employee)
        {
            if (employee == null)
                throw new ArgumentNullException(nameof(employee));

            //if (!Employees.Exists(e => e.Id == employee.Id))
            //Проверяем, чтобы сотрудник с таким ID не был в отделе
            foreach (var e in Employees)
            {
                if (e.Id == employee.Id)
                {
                    throw new ArgumentException("сотрудник с таким ID уже существует в отделе.");
                }
            }

            if (!Employees.Contains(employee))
            {
                Employees.Add(employee);
            }

            employee.Department = this; // ссылка на этот отдел

            // Если у отдела есть глава, добавляем сотрудника в подчинённые главы
            if (Head != null && employee.Id != Head.Id) // Проверяем, чтобы глава не был подчинённым самому себе
            {
                Head.AddSubordinate(employee);
            }
        }

        /// <summary>
        /// Удаление сотрудника
        /// </summary>
        /// <param name="employee"></param>
        /// <returns>Успешно?</returns>
        public bool RemoveEmployee(Employee employee)
        {
            // Если у отдела есть глава, удаляем сотрудника из подчинённых главы
            Head?.RemoveSubordinate(employee);

            return Employees.Remove(employee);
        }

        #endregion

        #region Методы для работы с бюджетом
        // Методы для работы с бюджетом

        /// <summary>
        /// Расчет общей зарплаты отдела
        /// </summary>
        /// <returns>Размер общей зарплаты</returns>
        private decimal CalculateTotalSalary()
        {
            decimal totalSalary = 0;

            for (int i = 0; i < Employees.Count; i++)
            {
                totalSalary += Employees[i].CalculateSalary();
            }

            return totalSalary;
        }

        /// <summary>
        /// Расчет остатка бюджета
        /// </summary>
        /// <returns>Остаток бюджета</returns>
        private decimal GetBudgetRemainder()
        {
            return Budget - CalculateTotalSalary();
        }

        #endregion

        #region Реализация интерфейса IReportable
        // Реализация интерфейса IReportable

        /// <summary>
        /// Генерация отчета
        /// </summary>
        /// <param name="format"></param>
        /// <returns>Строка отчета</returns>
        public string GenerateReport(string format)
        {
            if (format.ToLower() == "short")
            {
                return $"{Id,-5}{Name,-40}{Employees.Count,-12}{Head?.Name,-30}{Budget,-18:C2}{GetBudgetRemainder(),-18:C2}"
                //return $"{Id,-5}{Name,-40}{Employees.Count,-12}{Head?.Name,-30}{Budget, -20:C2}{GetBudgetRemainder(),-20:C2}"
                //return $"Department: {Name} (ID: {Id}), Employees: {Employees.Count}, Budget: {Budget:C2}"
                ;
            }

            // Если формат не "short", генерируем полный отчет
            string employeesReport = "Сотрудники:\n";
            for (int i = 0; i < Employees.Count; i++)
            {
                employeesReport += $" - {Employees[i].Name+";",-30}" +
                    $"Должность: {Employees[i].GetType().Name+ " ("+Employees[i].Position + ")",-40}" +
                    $"Зарплата: {Employees[i].CalculateSalary(),-20:C2}\n";
            }
            return $"Отдел ID: {Id}\nНазвание: {Name}\n" +
                $"Руководитель: {(Head != null ? Head.Name : "Не назначен")}\n" +
                $"Бюджет: {Budget:C2}\nСумма зарплат: {CalculateTotalSalary():C2}\n" +
                $"Остаток бюджета: {GetBudgetRemainder():C2}\n" +
                $"Количество сотрудников: {Employees.Count}\n{employeesReport}";
        }
        // Метод получения списка специалистов по специализации

        /// <summary>
        /// Получение списка специалистов по специализации
        /// </summary>
        /// <param name="specialization"></param>
        /// <returns>Список специалистов</returns>
        public IEnumerable<Specialist> GetSpecialistsBySpecialization(string specialization)
        {
            List<Specialist> specialists = new List<Specialist>();
            for (int i = 0; i < Employees.Count; i++)
            {
                if (Employees[i] is Specialist specialist &&
                    specialist.Specialization.Equals(specialization, StringComparison.OrdinalIgnoreCase))
                {
                    specialists.Add(specialist);
                }
            }
            return specialists;
        }
        /// <summary>
        /// Получение списка всех сотрудников
        /// </summary>
        /// <param name="showOrderNumber"></param>
        /// <returns>Строка содержащая список сотрудников</returns>
        public string GetAllEmployeeList(bool showOrderNumber = false)
        {
            StringBuilder sb = new StringBuilder();
            sb.AppendLine("СПИСОК ВСЕХ СОТРУДНИКОВ\n");
            string header = showOrderNumber
                ? $"{"№",-3}{"ID",-5}{"Имя",-30}{"Тип сотрудника",-20}{"Отдел",-30}{"Стаж",-10}{"Ставка",-10}"
                : $"{"ID",-5}{"Имя",-30}{"Тип сотрудника",-20}{"Отдел",-30}{"Стаж",-10}{"Ставка",-10}";
            sb.AppendLine(header);
            sb.AppendLine(new string('-', showOrderNumber ? 108 : 105));
            for (int i = 0; i < Employees.Count; i++)
            {
                var employee = Employees[i];
                string departmentName = employee.Department?.Name ?? "Нет отдела";
                string employeeInfo = $"{employee.Id,-5}{employee.Name,-30}{employee.GetType().Name,-20}{departmentName,-30}{employee.CalculateExperienceString(),-10}{employee.CalculateSalary(),-10}";
                if (showOrderNumber)
                {
                    employeeInfo = $"{i + 1,-3}" + employeeInfo;
                }
                sb.AppendLine(employeeInfo);
            }
            return sb.ToString();
        }
        #endregion
    }
}

using EmployeeManagementSystem.Core.Enums;
using EmployeeManagementSystem.Core.Interfaces;
using EmployeeManagementSystem.Core.Models.Structs;
using System;
using System.Runtime.InteropServices.ComTypes;

namespace EmployeeManagementSystem.Core.Models
{
    /// <summary>
    /// Базовый абстрактный класс сотрудника
    /// </summary>
    [Serializable]
    public abstract class Employee : IPayable, IReportable
    {
        #region Свойства
        public int Id { get; set; }
        public string Name { get; set; }
        private DateTime BirthDate { get; set; }
        private DateTime HireDate { get; set; }
        public decimal BaseSalary { get; set; }
        public EmployeePosition Position { get; private set; }
        public ContactInfo ContactInfo { private get; set; }
        public Department Department { get; set; }

        #endregion

        #region Конструктор
        /// <summary>
        /// Конструктор
        /// </summary>
        /// <param name="id"></param>
        /// <param name="name"></param>
        /// <param name="birthDate"></param>
        /// <param name="hireDate"></param>
        /// <param name="baseSalary"></param>
        /// <param name="position"></param>
        /// <param name="contactInfo"></param>
        /// <param name="department"></param>
        protected Employee(int id, string name, DateTime birthDate, DateTime hireDate, decimal baseSalary,
            EmployeePosition position, ContactInfo contactInfo,
            Department department)
        {
            Id = id;
            Name = name;
            BirthDate = birthDate;
            HireDate = hireDate;
            BaseSalary = baseSalary;
            Position = position;
            ContactInfo = contactInfo;
            Department = department;
            Department?.AddEmployee(this);
        }
        #endregion
        #region Методы интерфейса IPayable
        // Методы интерфейса IPayable

        /// <summary>
        /// Вычисление зарплаты
        /// </summary>
        /// <returns></returns>
        public abstract decimal CalculateSalary();

        #endregion
        #region Методы интерфейса IReportable

        // Методы интерфейса IReportable
        public virtual string GenerateReport(string format)
        {
            return format.ToLower() == "short"
                ? $"ID: {Id}, Имя: {Name}, Должность: {Position}"
                : $"ID: {Id} \nИмя: {Name} \nДата рождения: {BirthDate.ToShortDateString()} \n" +
                  $"Дата найма: {HireDate.ToShortDateString()} \nСтаж работы: {CalculateExperienceString()}" +
                  $"\nДолжность: {Position} \nОтдел: {Department?.Name}" +
                  $"\nКонтактная информация: {ContactInfo} \nОклад: {BaseSalary:C2} \n" +
                  $"Вознаграждение: {CalculateSalary():C2}";
        }

        #endregion
        #region Дополнительные методы
        // Дополнительные методы

        /// <summary>
        /// Вычисление опыта работы
        /// </summary>
        /// <returns>опыт работы в месяцах</returns>
        public int CalculateExperience()
        {
            //return (int) ((DateTime.Now - HireDate).TotalDays / 365.25);
            return (DateTime.Now.Year - HireDate.Year) * 12 + (DateTime.Now.Month - HireDate.Month);

            int months = (DateTime.Now.Year - HireDate.Year) * 12;
            months += DateTime.Now.Month - HireDate.Month;
            if (DateTime.Now.Day < HireDate.Day)
            {
                months--;
            }
            return months;
        }

        // Вычислить опыт работы в годах и месяцах

        /// <summary>
        /// Вычисление опыта работы в годах и месяцах
        /// </summary>
        /// <returns>опыт работы в годах и месяцах</returns>
        public string CalculateExperienceString()
        {
            int years = DateTime.Now.Year - HireDate.Year;
            int months = DateTime.Now.Month - HireDate.Month;
            if (months < 0)
            {
                years--;
                months += 12;
            }
            if (DateTime.Now.Day < HireDate.Day)
            {
                months--;
                if (months < 0)
                {
                    years--;
                    months += 12;
                }
            }
            return $"{years}{GetYearWord(years)},{months}m";
        }

        /// <summary>
        /// Назначение на должность
        /// </summary>
        /// <param name="newPosition"></param>
        public void ChangePosition(EmployeePosition newPosition)
        {
            Position = newPosition;
        }

        public void DisplayInfo()
        {
            Console.WriteLine(GenerateReport("full"));
        }

        /// <summary>
        /// Возвращает слово "л" или "г" в зависимости от количества лет (правильно склоняет слово "год" в зависимости от числа лет:
        /// </summary>
        /// <param name="years"></param>
        /// <returns>Строка "л" или "г"</returns>
        private string GetYearWord(int years)
        {
            if (years % 100 >= 11 && years % 100 <= 19)
                return "л";
            switch (years % 10)
            {
                case 1:
                    return "г";
                case 2:
                case 3:
                case 4:
                    return "г";
                default:
                    return "л";
            }
        }

        // TO-DO set EmployeeName

        #endregion
    }
}

using EmployeeManagementSystem.Core.Enums;
using EmployeeManagementSystem.Core.Interfaces;
using EmployeeManagementSystem.Core.Models.Structs;
using System;
using System.Collections.Generic;

namespace EmployeeManagementSystem.Core.Models
{
    /// <summary>
    /// Класс менеджер наследует от класса Employee
    /// </summary>
    [Serializable]
    public class Manager : Employee, IManagerial
    {
        #region Дополнительные свойства
        // Дополнительные свойства
        public List<Employee> Subordinates { get; private set; }
        public ManagementLevel ManagementLevel { get; set; }
        public decimal ManagementBonus { get; set; }
        #endregion

        #region Конструктор
        // Конструктор

        /// <summary>
        /// Инициализация менеджера
        /// </summary>
        /// <param name="id"></param>
        /// <param name="name"></param>
        /// <param name="birthDate"></param>
        /// <param name="hireDate"></param>
        /// <param name="baseSalary"></param>
        /// <param name="position"></param>
        /// <param name="contactInfo"></param>
        /// <param name="department"></param>
        /// <param name="managementLevel"></param>
        /// <param name="managementBonus"></param>
        public Manager(int id, string name, DateTime birthDate, DateTime hireDate, decimal baseSalary,
            EmployeePosition position, ContactInfo contactInfo, Department department,
            ManagementLevel managementLevel, decimal managementBonus)
            : base(id, name, birthDate, hireDate, baseSalary, position, contactInfo, department)
        {
            ManagementLevel = managementLevel;
            ManagementBonus = managementBonus;
            Subordinates = new List<Employee>();
        }

        #endregion

        #region Переопределение метода из IPayable
        // Переопределение метода из IPayable

        /// <summary>
        /// Вычисление зарплаты
        /// </summary>
        /// <returns>Зарплата с учётом бонусов</returns>
        public override decimal CalculateSalary()
        {
            decimal experienceBonus = BaseSalary * (CalculateExperience() / 100m);
            decimal subordinatesBonus = ManagementBonus * Subordinates.Count;
            return BaseSalary + experienceBonus + subordinatesBonus;
        }

        #endregion

        #region Реализация методов интерфейса IManagerial
        // Реализация методов интерфейса IManagerial

        /// <summary>
        /// Добавление подчиненного
        /// </summary>
        /// <param name="employee"></param>
        /// <returns>Статус добавления</returns>
        public bool AddSubordinate(Employee employee)
        {
            if (employee == null)
                throw new ArgumentNullException(nameof(employee));
            if (Id == employee.Id) // Менеджер не может быть подчинённым самому себе
            {
                return false;
            }

            Subordinates.Add(employee);
            return true;
        }

        /// <summary>
        /// Удаление подчиненного
        /// </summary>
        /// <param name="employeeId"></param>
        /// <returns>Статус удаления</returns>
        public bool RemoveSubordinate(Employee employee)
        {
            for (int i = 0; i < Subordinates.Count; i++)
            {
                if (Subordinates[i] == employee)
                {
                    Subordinates.RemoveAt(i);
                    Subordinates.Remove(employee);
                    return true;
                }
            }
            return false;
        }

        /// <summary>
        /// Получение подчиненных
        /// </summary>
        /// <returns></returns>
        public IEnumerable<Employee> GetSubordinates()
        {
            return Subordinates;
        }

        #endregion

        #region Переопределение методов из Employee
        // Переопределение методов из Employee

        /// <summary>
        /// Генерация отчета
        /// </summary>
        /// <param name="format"></param>
        /// <returns>Строка отчета</returns>
        public override string GenerateReport(string format)
        {
            string baseReport = base.GenerateReport(format);

            if (format.ToLower() == "short")
            {
                return $"{baseReport}, Уровень менеджмента: {ManagementLevel}, Подчиненных: {Subordinates.Count}";
            }

            string subordinatesReport = "Подчиненные:\n";
            for (int i = 0; i < Subordinates.Count; i++)
            {
                subordinatesReport += $" - {Subordinates[i].Name} (ID: {Subordinates[i].Id})\n";
            }
            return $"{baseReport}\nУровень менеджмента: {ManagementLevel}\n{subordinatesReport}";
        }

        #endregion

        #region Дополнительные методы
        // Дополнительные методы

        /// <summary>
        /// Вычисление зарплаты команды
        /// </summary>
        /// <returns>Зарплата команды</returns>
        public decimal CalculateTeamSalary()
        {
            decimal totalSalary = CalculateSalary();

            for (int i = 0; i < Subordinates.Count; i++)
            {
                totalSalary += Subordinates[i].CalculateSalary();
            }
            return totalSalary;
        }
        #endregion
    }
}

using EmployeeManagementSystem.Core.Enums;
using EmployeeManagementSystem.Core.Interfaces;
using System;
using System.Collections.Generic;

namespace EmployeeManagementSystem.Core.Models
{
    [Serializable]
    public class Project : IReportable
    {
        #region Свойства
        // Свойства
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public decimal Budget { get; set; }
        public ProjectStatus? Status { get; set; }
        public Manager ProjectManager { get; set; }
        public List<Employee> Team { get; private set; }

        #endregion

        #region Конструктор
        // Конструктор
        public Project(int id, string name, DateTime startDate, DateTime endDate, decimal budget,
            ProjectStatus? status, Manager projectManager)
        {
            Id = id;
            Name = name;
            StartDate = startDate;
            EndDate = endDate;
            Budget = budget;
            Status = status;
            ProjectManager = projectManager;
            Team = new List<Employee>();
            // Добавляем менеджера проекта в команду
            if (projectManager != null)
            {
                Team.Add(projectManager);
            }
        }

        #endregion

        #region Методы для управления командой и проектом
        // Методы для управления командой
        public void AssignTeamMember(Employee employee, decimal hoursPerWeek)
        {
            Team.Add(employee);

            // Если сотрудник - специалист, добавляем проект в его список
            if (employee is Specialist specialist)
            {
                specialist.AddProject(Id, this, hoursPerWeek);
            }
        }

        public bool RemoveTeamMember(int employeeId)
        {
            for (int i = 0; i < Team.Count; i++)
            {
                if (Team[i].Id == employeeId)
                {
                    // Если удаляем специалиста, то удаляем и назначение на проект
                    if (Team[i] is Specialist specialist)
                    {
                        specialist.RemoveProject(Id);
                    }
                    if (Team[i] == ProjectManager)
                    {
                        ProjectManager = null;
                    }
                    Team.RemoveAt(i);
                    return true;
                }
            }
            return false;
        }

        // Методы для управления проектом
        public void UpdateStatus(ProjectStatus newStatus)
        {
            Status = newStatus;
            if (newStatus == ProjectStatus.Completed)
            {
                EndDate = DateTime.Now;
            }
        }

        public int GetDuration()
        {
            if (Status == ProjectStatus.Completed)
            {
                return (EndDate - StartDate).Days;
            }
            return (DateTime.Now - StartDate).Days;
        }

        #endregion

        #region Реализация интерфейса IReportable
        // Реализация интерфейса IReportable
        public string GenerateReport(string format)
        {
            if (format.ToLower() == "short")
            {
                return $"Проект: {Name} (ID: {Id}), Статус: {Status}, Сотрудников в команде: {Team.Count}";
            }

            string teamReport = "Члены команды:\n";
            for (int i = 0; i < Team.Count; i++)
            {
                teamReport += $" - {Team[i].Name} (ID: {Team[i].Id})\n";
            }

            return $"Проект ID: {Id}\nНазвание: {Name}\n" +
                $"Дата начала: {StartDate.ToShortDateString()}\nДата окончания: {EndDate.ToShortDateString()}\n" +
                $"Статус: {Status}\nБюджет: {Budget:C2}\n" +
                $"Менеджер: {(ProjectManager != null ? ProjectManager.Name : "Не назначен")}\n" +
                $"Продолжительность: {GetDuration()} дней\n" +
                $"Сотрудников в команде: {Team.Count}\n{teamReport}";
        }

        #endregion
    }
}

using EmployeeManagementSystem.Core.Enums;
using EmployeeManagementSystem.Core.Models.Structs;
using System;
using System.Collections.Generic;

namespace EmployeeManagementSystem.Core.Models
{
    /// <summary>
    /// Класс специалист наследует от класса Employee
    /// </summary>
    [Serializable]
    public class Specialist : Employee
    {
        #region Свойства
        // Дополнительные свойства
        public string Specialization { get; set; }
        public int QualificationLevel { get; set; }
        public List<ProjectAssignment> ProjectAssignments { get; private set; }

        #endregion

        #region Конструктор
        // Конструктор
        /// <summary>
        /// Инициализация специалиста
        /// </summary>
        /// <param name="id"></param>
        /// <param name="name"></param>
        /// <param name="birthDate"></param>
        /// <param name="hireDate"></param>
        /// <param name="baseSalary"></param>
        /// <param name="position"></param>
        /// <param name="contactInfo"></param>
        /// <param name="department"></param>
        /// <param name="specialization"></param>
        /// <param name="qualificationLevel"></param>
        public Specialist(int id, string name, DateTime birthDate, DateTime hireDate, decimal baseSalary,
            EmployeePosition position, ContactInfo contactInfo,
            Department department,
            string specialization, int qualificationLevel)
            : base(id, name, birthDate, hireDate, baseSalary, position, contactInfo, department)
        {
            Specialization = specialization;
            QualificationLevel = qualificationLevel;
            ProjectAssignments = new List<ProjectAssignment>();
        }

        #endregion

        #region Переопределение метода из IPayable
        // Переопределение метода из IPayable

        /// <summary>
        /// Вычисление зарплаты
        /// </summary>
        /// <returns></returns>
        public override decimal CalculateSalary()
        {
            decimal experienceBonus = BaseSalary * (CalculateExperience() / 200m);
            decimal qualificationBonus = BaseSalary * (QualificationLevel / 10m);
            decimal projectsBonus = 0;

            for (int i = 0; i < ProjectAssignments.Count; i++)
            {
                projectsBonus += ProjectAssignments[i].HoursPerWeek * 0.2m;
            }

            return BaseSalary + experienceBonus + qualificationBonus + projectsBonus;
        }

        #endregion

        #region Методы для управления проектами
        // Методы для управления проектами

        /// <summary>
        /// Добавление проекта
        /// </summary>
        /// <param name="projectId"></param>
        /// <param name="hoursPerWeek"></param>
        public void AddProject(int projectId, Project project, decimal hoursPerWeek)
        {
            //if (!ProjectAssignments.Exists(p => p.ProjectId == projectId)
            ProjectAssignment assignment = new ProjectAssignment(
                projectId,
                project,
                Id,
                DateTime.Now,
                hoursPerWeek
            );
            ProjectAssignments.Add(assignment);
        }
        /// <summary>
        /// Удаление проекта
        /// </summary>
        /// <param name="projectId"></param>
        /// <returns></returns>
        public bool RemoveProject(int projectId)
        {
            for (int i = 0; i < ProjectAssignments.Count; i++)
            {
                if (ProjectAssignments[i].ProjectId == projectId)
                {
                    ProjectAssignments.RemoveAt(i);
                    return true;
                }
            }
            return false;
        }

        #endregion
        #region Метод повышения квалификации
        // Метод повышения квалификации

        /// <summary>
        /// Повышение квалификации
        /// </summary>
        /// <param name="newLevel"></param>
        public void UpdateQualification(int newLevel)
        {
            if (newLevel >= 1 && newLevel <= 10)
            {
                QualificationLevel = newLevel;
            }
        }

        #endregion

        #region Переопределение методов из Employee
        // Переопределение методов из Employee

        /// <summary>
        /// Генерация отчета
        /// </summary>
        /// <param name="format"></param>
        /// <returns>Строка отчета</returns>
        public override string GenerateReport(string format)
        {
            string baseReport = base.GenerateReport(format);

            if (format.ToLower() == "short")
            {
                return $"{baseReport}, Специализация: {Specialization}, Проекты: {ProjectAssignments.Count}";
            }

            string projectsReport = "Задачи по проектам:\n";
            for (int i = 0; i < ProjectAssignments.Count; i++)
            {
                //projectsReport += $" - Проект ID: {ProjectAssignments[i].ProjectId}, " +
                // $"Часов в неделю: {ProjectAssignments[i].HoursPerWeek}\n";
                projectsReport += $" - Проект: {ProjectAssignments[i].Project.Name}, " +
                    $"Часов в неделю: {ProjectAssignments[i].HoursPerWeek}\n";
            }
            return $"{baseReport}\nСпециализация: {Specialization}\n" +
                $"Квалификационный уровень: {QualificationLevel}\n{projectsReport}";
        }
        #endregion
    }
}

using EmployeeManagementSystem.Core.Models;
using System.Collections.Generic;

namespace EmployeeManagementSystem.Core.Interfaces
{
    /// <summary>
    /// Интерфейс для объектов с управленческими функциями
    /// </summary>
    public interface IManagerial
    {
        /// <summary>
        /// Добавление подчиненного
        /// </summary>
        /// <param name="employee">Сотрудник</param>
        bool AddSubordinate(Employee employee);
        /// <summary>
        /// Удаление подчиненного
        /// </summary>
        /// <param name="employed">ID сотрудника</param>
        /// <returns>Признак успешного удаления</returns>
        bool RemoveSubordinate(Employee employee);
        // bool RemoveSubordinate(int employeeId);
        /// <summary>
        /// Получение списка подчиненных
        /// </summary>
        /// <returns>Коллекция подчиненных</returns>
        IEnumerable<Employee> GetSubordinates();
    }
}

namespace EmployeeManagementSystem.Core.Interfaces
{
    /// <summary>
    /// Интерфейс для объектов, которым может начисляться оплата
    /// </summary>
    public interface IPayable
    {
        /// <summary>
        /// Расчет заработной платы
        /// </summary>
        /// <returns>Сумма к выплате</returns>
        decimal CalculateSalary();
    }
}

namespace EmployeeManagementSystem.Core.Interfaces
{
    /// <summary>
    /// Интерфейс для объектов, формирующих отчеты
    /// </summary>
    public interface IReportable
    {
        /// <summary>
        /// Создание отчета
        /// </summary>
        /// <param name="format">Формат отчета</param>
        /// <returns>Строка с отчетом в указанном формате</returns>
        string GenerateReport(string format);
    }
}

using Xunit;
using EmployeeManagementSystem.Core.Models;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using EmployeeManagementSystem.Core.Enums;
using EmployeeManagementSystem.Core.Models.Structs;
using EmployeeManagementSystem.Core.Services;

namespace EmployeeManagementSystem.Core.Models.Tests
{
    #region EmployeeTests
    public class EmployeeTests
    {
        private readonly ContactInfo _testContactInfo = new ContactInfo(
            "test@example.com",
            "+1234567890",
            "123 Test Street");

        [Fact]
        public void CalculateExperience_ShouldReturnCorrectMonths()
        {
            // Arrange
            var now = DateTime.Now;
            var hireDate = now.AddMonths(-15);

            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Doe", DateTime.Now.AddYears(-30), hireDate, 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            // Act
            int experience = specialist.CalculateExperience();

            // Assert
            Assert.Equal(15, experience);
        }

        [Fact]
        public void CalculateExperienceString_ShouldReturnCorrectFormat()
        {
            // Arrange
            var now = DateTime.Now;
            var hireDate = now.AddYears(-1).AddMonths(-3);

            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Doe", DateTime.Now.AddYears(-30), hireDate, 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            // Act
            string experienceString = specialist.CalculateExperienceString();

            // Assert
            Assert.Equal("1г,3м", experienceString);
        }
    }
    #endregion

    #region ManagerTests
    public class ManagerTests
    {
        private readonly ContactInfo _testContactInfo = new ContactInfo(
            "test@example.com",
            "+1234567890",
            "123 Test Street");

        [Fact]
        public void AddSubordinate_ShouldAddEmployeeToList()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var specialist = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            // Act
            var result = manager.AddSubordinate(specialist);

            // Assert
            Assert.True(result);
            Assert.Single(manager.Subordinates);
            Assert.Equal(specialist, manager.Subordinates[0]);
        }

        [Fact]
        public void AddSubordinate_ShouldReturnFalse_WhenAddingSelf()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            // Act
            var result = manager.AddSubordinate(manager);

            // Assert
            Assert.False(result);
            Assert.Empty(manager.Subordinates);
        }

        [Fact]
        public void CalculateSalary_ShouldIncludeExperienceAndSubordinatesBonus()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddMonths(-60), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var specialist1 = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            var specialist2 = new Specialist(
                3, "Alice Developer", DateTime.Now.AddYears(-25),
                DateTime.Now.AddYears(-1), 4500,
                EmployeePosition.MiddleSpecialist, _testContactInfo, department,
                "Software Development", 6);

            manager.AddSubordinate(specialist1);
            manager.AddSubordinate(specialist2);

            // Act
            decimal salary = manager.CalculateSalary();

            // Assert
            decimal expectedExperienceBonus = 10000 * (60m / 100m); // 60 месяцев / 100
            decimal expectedSubordinatesBonus = 500 * 2; // 500 per subordinate
            decimal expectedSalary = 10000 + expectedExperienceBonus + expectedSubordinatesBonus;

            Assert.Equal(expectedSalary, salary);
        }

        [Fact]
        public void RemoveSubordinate_ShouldRemoveEmployeeFromList()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var specialist = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            manager.AddSubordinate(specialist);

            // Act
            var result = manager.RemoveSubordinate(specialist);

            // Assert
            Assert.True(result);
            Assert.Empty(manager.Subordinates);
        }

        [Fact]
        public void CalculateTeamSalary_ShouldSumManagerAndSubordinatesSalaries()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var specialist = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            manager.AddSubordinate(specialist);

            // Act
            decimal teamSalary = manager.CalculateTeamSalary();

            // Assert
            decimal managerSalary = manager.CalculateSalary();
            decimal specialistSalary = specialist.CalculateSalary();

            Assert.Equal(managerSalary + specialistSalary, teamSalary);
        }
    }
    #endregion

    #region SpecialistTests
    public class SpecialistTests
    {
        private readonly ContactInfo _testContactInfo = new ContactInfo(
            "test@example.com",
            "+1234567890",
            "123 Test Street");

        [Fact]
        public void CalculateSalary_ShouldIncludeQualificationAndProjectBonuses()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddMonths(-24), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            specialist.AddProject(1, null, 20); // 20 hours per week
            specialist.AddProject(2, null, 10); // 10 hours per week

            // Act
            decimal salary = specialist.CalculateSalary();

            // Assert
            decimal expectedExperienceBonus = 5000 * (24m / 200m); // 24 months / 200
            decimal expectedQualificationBonus = 5000 * (8m / 10m); // Level 8 / 10
            decimal expectedProjectBonus = (20 + 10) * 0.2m; // 30 hours * 0.2
            decimal expectedSalary = 5000 + expectedExperienceBonus + expectedQualificationBonus + expectedProjectBonus;

            Assert.Equal(expectedSalary, salary);
        }

        [Fact]
        public void AddProject_ShouldAddToProjectAssignments()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            // Act
            specialist.AddProject(1, null, 20);

            // Assert
            Assert.Single(specialist.ProjectAssignments);
            Assert.Equal(1, specialist.ProjectAssignments[0].ProjectId);
            Assert.Equal(20, specialist.ProjectAssignments[0].HoursPerWeek);
        }

        [Fact]
        public void RemoveProject_ShouldRemoveFromProjectAssignments()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            specialist.AddProject(1, null, 20);
            specialist.AddProject(2, null, 10);

            // Act
            var result = specialist.RemoveProject(1);

            // Assert
            Assert.True(result);
            Assert.Single(specialist.ProjectAssignments);
            Assert.Equal(2, specialist.ProjectAssignments[0].ProjectId);
        }

        [Fact]
        public void UpdateQualification_ShouldChangeQualificationLevel()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 5);

            // Act
            specialist.UpdateQualification(9);

            // Assert
            Assert.Equal(9, specialist.QualificationLevel);
        }
    }
    #endregion

    #region DepartmentTests
    public class DepartmentTests
    {
        private readonly ContactInfo _testContactInfo = new ContactInfo(
            "test@example.com",
            "+1234567890",
            "123 Test Street");

        [Fact]
        public void AddEmployee_ShouldAddToEmployeeList()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, null,
                "Software Development", 8);

            // Act
            department.AddEmployee(specialist);

            // Assert
            Assert.Single(department.Employees);
            Assert.Equal(specialist, department.Employees[0]);
            Assert.Equal(department, specialist.Department);
        }

        [Fact]
        public void SetHead_ShouldUpdateHeadAndAddToSubordinates()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, null,
                ManagementLevel.MiddleManager, 500);

            var specialist = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, null,
                "Software Development", 8);

            department.AddEmployee(specialist);

            // Act
            department.SetHead(manager);

            // Assert
            Assert.Equal(manager, department.Head);
            Assert.Contains(manager, department.Employees);
            Assert.Contains(specialist, manager.Subordinates);
        }

        [Fact]
        public void RemoveEmployee_ShouldRemoveFromEmployeeList()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var specialist = new Specialist(
                1, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, null,
                "Software Development", 8);

            var manager = new Manager(
                2, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, null,
                ManagementLevel.MiddleManager, 500);

            department.AddEmployee(manager);
            department.SetHead(manager);

            department.AddEmployee(specialist);

            // Act
            //var result = department.RemoveEmployee(1);
            var result = department.RemoveEmployee(specialist);

            // Assert
            Assert.True(result);

            result = department.RemoveEmployee(manager);
            Assert.True(result);

            Assert.Empty(department.Employees);
        }

        [Fact]
        public void GetSpecialistsBySpecialization_ShouldReturnMatchingSpecialists()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);

            var specialist1 = new Specialist(
                1, "John Developer", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, null,
                "Software Development", 8);

            var specialist2 = new Specialist(
                2, "Alice Developer", DateTime.Now.AddYears(-28),
                DateTime.Now.AddYears(-1), 4500,
                EmployeePosition.MiddleSpecialist, _testContactInfo, null,
                "Software Development", 6);

            var specialist3 = new Specialist(
                3, "Bob Designer", DateTime.Now.AddYears(-25),
                DateTime.Now.AddMonths(-6), 4000,
                EmployeePosition.JuniorSpecialist, _testContactInfo, null,
                "UI/UX Design", 4);

            department.AddEmployee(specialist1);
            department.AddEmployee(specialist2);
            department.AddEmployee(specialist3);

            // Act
            var devSpecialists = department.GetSpecialistsBySpecialization("Software Development").ToList();

            // Assert
            Assert.Equal(2, devSpecialists.Count);
            Assert.Contains(specialist1, devSpecialists);
            Assert.Contains(specialist2, devSpecialists);
        }
    }
    #endregion

    #region ProjectTests
    public class ProjectTests
    {
        private readonly ContactInfo _testContactInfo = new ContactInfo(
            "test@example.com",
            "+1234567890",
            "123 Test Street");

        [Fact]
        public void AssignTeamMember_ShouldAddEmployeeToTeam()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var project = new Project(
                1, "Test Project", DateTime.Now, DateTime.Now.AddMonths(6), 50000,
                ProjectStatus.Planning, manager);

            var specialist = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            // Act
            project.AssignTeamMember(specialist, 20);

            // Assert
            Assert.Equal(2, project.Team.Count); // Manager + Specialist
            Assert.Contains(specialist, project.Team);

            // Check that the project was added to specialist's assignments
            Assert.Single(specialist.ProjectAssignments);
            Assert.Equal(1, specialist.ProjectAssignments[0].ProjectId);
            Assert.Equal(20, specialist.ProjectAssignments[0].HoursPerWeek);
        }

        [Fact]
        public void RemoveTeamMember_ShouldRemoveEmployeeFromTeam()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var project = new Project(
                1, "Test Project", DateTime.Now, DateTime.Now.AddMonths(6), 50000,
                ProjectStatus.Planning, manager);

            var specialist = new Specialist(
                2, "John Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "Software Development", 8);

            project.AssignTeamMember(specialist, 20);

            // Act
            var result = project.RemoveTeamMember(2);

            // Assert
            Assert.True(result);
            Assert.Single(project.Team); // Only the manager remains
            Assert.DoesNotContain(specialist, project.Team);

            // Check that the project was removed from specialist's assignments
            Assert.Empty(specialist.ProjectAssignments);
        }

        [Fact]
        public void UpdateStatus_ShouldChangeProjectStatus()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var project = new Project(
                1, "Test Project", DateTime.Now.AddDays(-30),
                DateTime.Now.AddMonths(6), 50000,
                ProjectStatus.Planning, manager);

            // Act
            project.UpdateStatus(ProjectStatus.InProgress);

            // Assert
            Assert.Equal(ProjectStatus.InProgress, project.Status);
        }

        [Fact]
        public void UpdateStatus_Completed_ShouldSetEndDateToNow()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var originalEndDate = DateTime.Now.AddMonths(6);
            var project = new Project(
                1, "Test Project", DateTime.Now.AddDays(-30), originalEndDate, 50000,
                ProjectStatus.InProgress, manager);

            // Act
            project.UpdateStatus(ProjectStatus.Completed);

            // Assert
            Assert.Equal(ProjectStatus.Completed, project.Status);
            Assert.NotEqual(originalEndDate, project.EndDate);

            // The end date should be close to now (within a second)
            var difference = Math.Abs((DateTime.Now - project.EndDate).TotalSeconds);
            Assert.True(difference < 1);
        }

        [Fact]
        public void GetDuration_ShouldReturnCorrectNumberOfDays()
        {
            // Arrange
            var department = new Department(1, "Test Department", null, 100000);
            var manager = new Manager(
                1, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var startDate = DateTime.Now.AddDays(-30);
            var project = new Project(
                1, "Test Project", startDate, DateTime.Now.AddMonths(6), 50000,
                ProjectStatus.InProgress, manager);

            // Act
            int duration = project.GetDuration();

            // Assert
            Assert.Equal(30, duration);
        }
    }
    #endregion

    #region CompanyTests
    public class CompanyTests
    {
        private readonly ContactInfo _testContactInfo = new ContactInfo(
            "test@example.com",
            "+1234567890",
            "123 Test Street");

        [Fact]
        public void AddDepartment_ShouldAddToDepartmentsList()
        {
            // Arrange
            var ceo = new Manager(
                1, "John CEO", DateTime.Now.AddYears(-50), DateTime.Now.AddYears(-10), 20000,
                EmployeePosition.CEO, _testContactInfo, null,
                ManagementLevel.TopManager, 1000);

            var company = new Company("Test Company", ceo);
            var department = new Department(1, "HR Department", null, 100000);

            // Act
            company.AddDepartment(department);

            // Assert
            Assert.Single(company.Departments);
            Assert.Equal(department, company.Departments[0]);
        }

        [Fact]
        public void RemoveDepartment_ShouldRemoveFromDepartmentsList()
        {
            // Arrange
            var ceo = new Manager(
                1, "John CEO", DateTime.Now.AddYears(-50), DateTime.Now.AddYears(-10), 20000,
                EmployeePosition.CEO, _testContactInfo, null,
                ManagementLevel.TopManager, 1000);

            var company = new Company("Test Company", ceo);
            var department = new Department(1, "HR Department", null, 100000);
            company.AddDepartment(department);

            var developer1 = new Specialist(
                5, "Charlie Dev", DateTime.Now.AddYears(-30), DateTime.Now.AddYears(-2), 8000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, null,
                "Software Development", 8);

            var developer2 = new Specialist(
                6, "Dave Dev", DateTime.Now.AddYears(-28), DateTime.Now.AddYears(-1), 6000,
                EmployeePosition.MiddleSpecialist, _testContactInfo, null,
                "Software Development", 6);

            var manager = new Manager(
                7, "Jane Manager", DateTime.Now.AddYears(-40), DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, null,
                ManagementLevel.MiddleManager, 500);

            department.SetHead(manager);
            department.AddEmployee(developer1);
            department.AddEmployee(developer2);

            // Act
            var result = company.RemoveDepartment(department);

            // Assert
            Assert.False(result); // Department is not empty

            department.RemoveEmployee(developer1);
            department.RemoveEmployee(developer2);
            department.RemoveEmployee(manager);

            // Remove empty department
            result = company.RemoveDepartment(department);

            // Assert
            Assert.True(result);
            Assert.Empty(company.Departments);
        }

        [Fact]
        public void AddProject_ShouldAddToProjectsList()
        {
            // Arrange
            var ceo = new Manager(
                1, "John CEO", DateTime.Now.AddYears(-50), DateTime.Now.AddYears(-10), 20000,
                EmployeePosition.CEO, _testContactInfo, null,
                ManagementLevel.TopManager, 1000);

            var company = new Company("Test Company", ceo);
            var department = new Department(1, "IT Department", null, 200000);

            var manager = new Manager(
                2, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var project = new Project(
                1, "New System", DateTime.Now, DateTime.Now.AddMonths(6), 50000,
                ProjectStatus.Planning, manager);

            // Act
            company.AddProject(project);

            // Assert
            Assert.Single(company.Projects);
            Assert.Equal(project, company.Projects[0]);
        }

        [Fact]
        public void RemoveProject_ShouldRemoveFromProjectsList()
        {
            // Arrange
            var ceo = new Manager(
                1, "John CEO", DateTime.Now.AddYears(-50), DateTime.Now.AddYears(-10), 20000,
                EmployeePosition.CEO, _testContactInfo, null,
                ManagementLevel.TopManager, 1000);

            var company = new Company("Test Company", ceo);
            var department = new Department(1, "IT Department", null, 200000);

            var manager = new Manager(
                2, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var project = new Project(
                1, "New System", DateTime.Now, DateTime.Now.AddMonths(6), 50000,
                ProjectStatus.Planning, manager);

            company.AddProject(project);

            // Act
            var result = company.RemoveProject(1);

            // Assert
            Assert.True(result);
            Assert.Empty(company.Projects);
        }

        [Fact]
        public void GetTotalEmployees_ShouldReturnCorrectCount()
        {
            // Arrange
            var ceo = new Manager(
                1, "John CEO", DateTime.Now.AddYears(-50), DateTime.Now.AddYears(-10), 20000,
                EmployeePosition.CEO, _testContactInfo, null,
                ManagementLevel.TopManager, 1000);

            var company = new Company("Test Company", ceo);

            var hrDepartment = new Department(1, "HR Department", null, 100000);
            var itDepartment = new Department(2, "IT Department", null, 200000);

            var hrManager = new Manager(
                2, "Jane HR", DateTime.Now.AddYears(-45), DateTime.Now.AddYears(-7), 12000,
                EmployeePosition.DepartmentHead, _testContactInfo, hrDepartment,
                ManagementLevel.MiddleManager, 600);

            var itManager = new Manager(
                3, "Bob IT", DateTime.Now.AddYears(-40), DateTime.Now.AddYears(-5), 15000,
                EmployeePosition.DepartmentHead, _testContactInfo, itDepartment,
                ManagementLevel.MiddleManager, 700);

            var hrSpecialist = new Specialist(
                4, "Alice HR", DateTime.Now.AddYears(-35), DateTime.Now.AddYears(-3), 7000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, hrDepartment,
                "HR Management", 7);

            var developer1 = new Specialist(
                5, "Charlie Dev", DateTime.Now.AddYears(-30), DateTime.Now.AddYears(-2), 8000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, itDepartment,
                "Software Development", 8);

            var developer2 = new Specialist(
                6, "Dave Dev", DateTime.Now.AddYears(-28), DateTime.Now.AddYears(-1), 6000,
                EmployeePosition.MiddleSpecialist, _testContactInfo, itDepartment,
                "Software Development", 6);

            hrDepartment.SetHead(hrManager);
            itDepartment.SetHead(itManager);

            company.AddDepartment(hrDepartment);
            company.AddDepartment(itDepartment);

            // Act
            int totalEmployees = company.GetTotalEmployees();

            // Assert
            Assert.Equal(6, totalEmployees); // CEO + 2 managers + 3 specialists
        }

        [Fact]
        public void GetAllEmployees_ShouldReturnAllEmployees()
        {
            // Arrange
            var ceo = new Manager(
                1, "John CEO", DateTime.Now.AddYears(-50), DateTime.Now.AddYears(-10), 20000,
                EmployeePosition.CEO, _testContactInfo, null,
                ManagementLevel.TopManager, 1000);

            var company = new Company("Test Company", ceo);

            var department = new Department(1, "HR Department", null, 100000);

            var manager = new Manager(
                2, "Jane Manager", DateTime.Now.AddYears(-40),
                DateTime.Now.AddYears(-5), 10000,
                EmployeePosition.DepartmentHead, _testContactInfo, department,
                ManagementLevel.MiddleManager, 500);

            var specialist = new Specialist(
                3, "Alice Specialist", DateTime.Now.AddYears(-30),
                DateTime.Now.AddYears(-2), 5000,
                EmployeePosition.SeniorSpecialist, _testContactInfo, department,
                "HR Management", 8);

            company.AddDepartment(department);

            department.SetHead(manager);
            //department.AddEmployee(specialist);

            // Act
            var allEmployees = company.GetAllEmployees().ToList();

            // Assert
            Assert.Equal(3, allEmployees.Count); // CEO + Manager + Specialist
            Assert.Contains(ceo, allEmployees);
            Assert.Contains(manager, allEmployees);
            Assert.Contains(specialist, allEmployees);
        }
    }
    #endregion

    #region DataServiceTests
    public class DataServiceTests
    {
        [Fact]
        public void SaveAndLoad_ShouldPreserveCompanyData()
        {
            var dataService = new DataService();
            var company = new Company("Test Company", null);
            var department = new Department(1, "Test Dept", null, 100000m);
            company.AddDepartment(department);

            dataService.SaveData(company, "test.bin");
            var loaded = dataService.LoadData("test.bin");

            Assert.NotNull(loaded);
            Assert.Equal(company.Name, loaded.Name);
            Assert.Equal(company.Departments.Count, loaded.Departments.Count);
        }
    }
    #endregion
}
