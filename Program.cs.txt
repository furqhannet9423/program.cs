using System;

namespace DataMunging
{
    internal class Program
    {
        static void Main(string[] args)
        {
            WeatherProgram();
            FootBallProgram();
        }

        static void FootBallProgram()
        {
            var footballTable = ReadTable("c:\\football.dat", new List<int> { 3, 7, 23, 29, 33, 37, 43, 50, 56 });
            PrintMinSpread(footballTable, 6, 7, 1);
        }

        static void WeatherProgram()
        {
            var weatherTable = ReadTable("c:\\weather.dat", new List<int> { 2, 6, 12,18 });
            //remove last
            weatherTable.RemoveAt(weatherTable.Count - 1);
            PrintMinSpread(weatherTable, 1, 2, 0);
        }

        private static void PrintMinSpread(List<List<string>> Table, int index1, int index2, int indexName )
        {
            var min = float.MaxValue;
            int index = 0;
            for (int i = 0; i < Table.Count; i++)
            {
                List<string>? cols = Table[i];
                var spread = Spread(cols[index1], cols[index2]);
                if (spread < min)
                {
                    min = spread;
                    index = i;
                }
            }
            var row = Table[index];
            Console.WriteLine($"{row[indexName]} {min}");
        }

        static float Spread(string col1, string col2)
        {
            var c1 = float.Parse(col1);
            var c2 = float.Parse(col2);
            return c1 > c2 ? c1 - c2 : c2 - c1;
        }

        static List<List<string>> ReadTable(string path, List<int> ColumnStart)
        {
            var result = new List<List<string>>();
            foreach (var rows in File.ReadAllLines(path))
            {
                if (rows.Any())
                {
                    var row = new List<string>();
                    for (int x = 0; x < ColumnStart.Count - 1; x++)
                    {
                        var val = rows.Substring(ColumnStart[x], ColumnStart[x + 1] - ColumnStart[x]).Replace("-", "").Replace("*", "").Trim();
                        if (!string.IsNullOrEmpty(val))
                            row.Add(val);
                    }
                    if (row.Count > 0)
                        result.Add(row);
                }
            }
            //remove header
            result.RemoveAt(0);
            return result;
        }
    }
}