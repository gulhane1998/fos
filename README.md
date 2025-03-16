using System;
using System.Collections.Generic;
using System.IO;

public class Invoice
{
    public string InvoiceNumber { get; set; }
    public DateTime InvoiceDate { get; set; }
    public string CustomerName { get; set; }
    public List<InvoiceItem> Items { get; set; }
    public decimal TotalAmount 
    { 
        get 
        {
            decimal total = 0;
            foreach (var item in Items)
            {
                total += item.TotalPrice;
            }
            return total;
        }
    }

    public Invoice(string invoiceNumber, DateTime invoiceDate, string customerName)
    {
        InvoiceNumber = invoiceNumber;
        InvoiceDate = invoiceDate;
        CustomerName = customerName;
        Items = new List<InvoiceItem>();
    }

    public void AddItem(string description, int quantity, decimal unitPrice)
    {
        Items.Add(new InvoiceItem(description, quantity, unitPrice));
    }

    public void GenerateInvoice(string filePath)
    {
        using (StreamWriter writer = new StreamWriter(filePath))
        {
            writer.WriteLine($"Invoice Number: {InvoiceNumber}");
            writer.WriteLine($"Invoice Date: {InvoiceDate.ToShortDateString()}");
            writer.WriteLine($"Customer Name: {CustomerName}");
            writer.WriteLine();
            writer.WriteLine("Items:");
            writer.WriteLine("Description\tQuantity\tUnit Price\tTotal Price");

            foreach (var item in Items)
            {
                writer.WriteLine($"{item.Description}\t{item.Quantity}\t{item.UnitPrice:C}\t{item.TotalPrice:C}");
            }

            writer.WriteLine();
            writer.WriteLine($"Total Amount: {TotalAmount:C}");
        }
    }
}

public class InvoiceItem
{
    public string Description { get; set; }
    public int Quantity { get; set; }
    public decimal UnitPrice { get; set; }
    public decimal TotalPrice 
    { 
        get 
        {
            return Quantity * UnitPrice;
        }
    }

    public InvoiceItem(string description, int quantity, decimal unitPrice)
    {
        Description = description;
        Quantity = quantity;
        UnitPrice = unitPrice;
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        Invoice invoice = new Invoice("INV001", DateTime.Now, "John Doe");
        invoice.AddItem("Item 1", 2, 50.00m);
        invoice.AddItem("Item 2", 1, 100.00m);
        invoice.AddItem("Item 3", 5, 20.00m);

        string filePath = "Invoice.txt";
        invoice.GenerateInvoice(filePath);

        Console.WriteLine($"Invoice generated successfully at {filePath}");
    }
}
