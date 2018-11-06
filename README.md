using System;
using System.Collections.Generic;
using System.Drawing;

namespace RoutePlanning
{
    public static class PathFinderTask
    {
        public static double CalculateDistance(this Point a, Point b)
        {
            var dx = a.X - b.X;
            var dy = a.Y - b.Y;
            return Math.Sqrt(dx * dx + dy * dy);
        }
        public static double Min = int.MaxValue;
        static void Evaluate(int[] permutation, Point[] checkpoints, int[] bestOrder)
        {
            //double min = double.MaxValue;
            double distance = 0;
            for (int i = 1; i < permutation.Length; i++)
            {
                distance += CalculateDistance(checkpoints[permutation[i - 1]], checkpoints[permutation[i]]);
            }
            if (distance < Min)
            {
                Min = distance;
                for (int j = 0; j < permutation.Length; j++)
                    bestOrder[j] = permutation[j];
            }
            return;
        }
        public static int[] FindBestCheckpointsOrder(Point[] checkpoints)
        {
            var bestOrder = new int[checkpoints.Length];
            var permutation = new int[checkpoints.Length];
            MakePermutations(permutation, checkpoints, 1, bestOrder);
            Min = int.MaxValue;
            return bestOrder;
        }

        private static int[] MakeTrivialPermutation(int size)
        {
            var bestOrder = new int[size];
            for (int i = 0; i < bestOrder.Length; i++)
                bestOrder[i] = i;
            return bestOrder;
        }

        static void MakePermutations(int[] permutation, Point[] checkpoints, int position, int[] bestOrder)
        {
            if (position == permutation.Length)
            {
                Evaluate(permutation, checkpoints, bestOrder);
                return;
            }
            for (int i = 1; i < permutation.Length; i++)
            {
                var index = Array.IndexOf(permutation, i, 0, position);
                if (index != -1)
                    continue;
                permutation[position] = i;
                MakePermutations(permutation, checkpoints, position + 1, bestOrder);
            }
        }
    }
}
