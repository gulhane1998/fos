WAP in C# to show the use of the GOTO and BREAK Statement.

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace ConsoleApplication32
{
    class Program
    {
        static void Main(string[] args)
        {
            string fruit = "Apple";
            switch (fruit)
            { 
                case"Banana":
                Console.Write(fruit+"is Delicious fruit");
             break;
             case"Chair":
                Console.Write(fruit+"is Delicious fruit");
             break; 
             case"Apple":
                     goto case "Banana";
             break; 
             case"Table":
                    goto case "Chair";
             break; 
            }
                Console.ReadKey();
        }
    }
}
