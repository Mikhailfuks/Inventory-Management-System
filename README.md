using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        Inventory inventory = new Inventory();
        bool running = true;

        while (running)
        {
            Console.Clear();
            Console.WriteLine("Система управления инвентарем");
            Console.WriteLine("1. Добавить предмет");
            Console.WriteLine("2. Удалить предмет");
            Console.WriteLine("3. Показать инвентарь");
            Console.WriteLine("4. Выход");
            Console.Write("Выберите действие: ");

            switch (Console.ReadLine())
            {
                case "1":
                    AddItem(inventory);
                    break;
                case "2":
                    RemoveItem(inventory);
                    break;
                case "3":
                    ShowInventory(inventory);
                    break;
                case "4":
                    running = false;
                    break;
                default:
                    Console.WriteLine("Неверный выбор. Пожалуйста, попробуйте снова.");
                    break;
            }
            Console.WriteLine("Нажмите любую клавишу для продолжения...");
            Console.ReadKey();
        }
    }

    static void AddItem(Inventory inventory)
    {
        Console.Write("Введите название предмета: ");
        string name = Console.ReadLine();
        Console.Write("Введите количество: ");
        int quantity;
        while (!int.TryParse(Console.ReadLine(), out quantity) || quantity <= 0)
        {
            Console.Write("Введите корректное количество: ");
        }

        inventory.AddItem(new Item(name, quantity));
        Console.WriteLine("Предмет добавлен.");
    }

    static void RemoveItem(Inventory inventory)
    {
        Console.Write("Введите название предмета для удаления: ");
        string name = Console.ReadLine();
        inventory.RemoveItem(name);
    }

    static void ShowInventory(Inventory inventory)
    {
        Console.WriteLine("Инвентарь:");
        foreach (var item in inventory.GetItems())
        {
            Console.WriteLine($"{item.Name} (количество: {item.Quantity})");
        }
    }
}

public class Item
{
    public string Name { get; }
    public int Quantity { get; set; }

    public Item(string name, int quantity)
    {
        Name = name;
        Quantity = quantity;
    }
}

public class Inventory
{
    private List<Item> items;

    public Inventory()
    {
        items = new List<Item>();
    }

    public void AddItem(Item newItem)
    {
        Item existingItem = items.Find(item => item.Name.Equals(newItem.Name, StringComparison.OrdinalIgnoreCase));
        if (existingItem != null)
        {
            existingItem.Quantity += newItem.Quantity;
        }
        else
        {
            items.Add(newItem);
        }
    }

    public void RemoveItem(string name)
    {


        Item existingItem = items.Find(item => item.Name.Equals(name, StringComparison.OrdinalIgnoreCase));
        if (existingItem != null)
        {
            items.Remove(existingItem);
            Console.WriteLine("Предмет удален.");
        }
        else
        {
            Console.WriteLine("Предмет не найден.");
        }
    }

    public List<Item> GetItems()
    {
        return items;
    }
}
