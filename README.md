# PoolCalculate-Template-
A template for the Pool Calculator project.
using System;

namespace PoolCalculator
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.Title = "Pool Calculator by Opia Odafe George";
            Console.Write("Enter your name: ");
            string userName = Console.ReadLine();

            bool keepRunning = true;
            while (keepRunning)
            {
                Console.Clear();
                Console.WriteLine("===== POOL CALCULATION SYSTEM =====");
                Console.WriteLine("Choose Pool Shape:");
                Console.WriteLine("1. Rectangular");
                Console.WriteLine("2. Circular");
                Console.Write("Enter option (1 or 2): ");
                string option = Console.ReadLine();

                Pool pool = null;

                if (option == "1")
                {
                    pool = new RectangularPool();
                }
                else if (option == "2")
                {
                    pool = new CircularPool();
                }
                else
                {
                    Console.WriteLine("Invalid option. Try again.");
                    continue;
                }

                pool.CollectDimensions();
                pool.CalculateArea();
                pool.CalculateVolume();

                Console.WriteLine("\n========= INVOICE =========");
                Console.WriteLine($"User: {userName}");
                Console.WriteLine($"Date/Time: {DateTime.Now}");
                pool.PrintDetails();
                Console.WriteLine("============================\n");

                Console.Write("Do you want to calculate another pool? (y/n): ");
                string response = Console.ReadLine();
                if (response.ToLower() != "y")
                {
                    keepRunning = false;
                }
            }

            Console.WriteLine("Thank you for using the Pool Calculator!");
        }
    }

    abstract class Pool
    {
        public float shallowDepth;
        public float deepDepth;
        public float avgDepth;
        public float area;
        public float volume;

        public abstract void CollectDimensions();
        public abstract void CalculateArea();
        public abstract void CalculateVolume();
        public abstract void PrintDetails();

        protected float GetPositiveFloat(string message)
        {
            float value;
            do
            {
                Console.Write(message);
            } while (!float.TryParse(Console.ReadLine(), out value) || value <= 0);
            return value;
        }
    }

    class RectangularPool : Pool
    {
        private float length;
        private float width;

        public override void CollectDimensions()
        {
            Console.WriteLine("\n-- Enter Rectangular Pool Dimensions (in meters) --");
            length = GetPositiveFloat("Length: ");
            width = GetPositiveFloat("Width: ");
            shallowDepth = GetPositiveFloat("Shallow End Depth: ");
            deepDepth = GetPositiveFloat("Deep End Depth: ");
            avgDepth = (shallowDepth + deepDepth) / 2;
        }

        public override void CalculateArea()
        {
            area = 2 * (length * width + length * avgDepth + width * avgDepth);
        }

        public override void CalculateVolume()
        {
            volume = length * width * avgDepth;
        }

        public override void PrintDetails()
        {
            Console.WriteLine("Pool Shape: Rectangular");
            Console.WriteLine($"Length: {length} m");
            Console.WriteLine($"Width: {width} m");
            Console.WriteLine($"Shallow Depth: {shallowDepth} m");
            Console.WriteLine($"Deep Depth: {deepDepth} m");
            Console.WriteLine($"Interior Surface Area: {area:F2} m²");
            Console.WriteLine($"Volume: {volume:F2} m³");
        }
    }

    class CircularPool : Pool
    {
        private float radius;

        public override void CollectDimensions()
        {
            Console.WriteLine("\n-- Enter Circular Pool Dimensions (in meters) --");
            radius = GetPositiveFloat("Radius: ");
            shallowDepth = GetPositiveFloat("Shallow End Depth: ");
            deepDepth = GetPositiveFloat("Deep End Depth: ");
            avgDepth = (shallowDepth + deepDepth) / 2;
        }

        public override void CalculateArea()
        {
            area = 2 * (MathF.PI * radius * radius) + (2 * MathF.PI * radius * avgDepth);
        }

        public override void CalculateVolume()
        {
            volume = MathF.PI * radius * radius * avgDepth;
        }

        public override void PrintDetails()
        {
            Console.WriteLine("Pool Shape: Circular");
            Console.WriteLine($"Radius: {radius} m");
            Console.WriteLine($"Shallow Depth: {shallowDepth} m");
            Console.WriteLine($"Deep Depth: {deepDepth} m");
            Console.WriteLine($"Interior Surface Area: {area:F2} m²");
            Console.WriteLine($"Volume: {volume:F2} m³");
        }
    }
}
