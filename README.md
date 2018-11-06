# -using System;
using System.Collections.Generic;
using System.Drawing;

namespace RoutePlanning
{
	public static class PathFinderTask
	{
        public static double GetDistance(Point prevPoint, Point nextPoint)
        {
            var dx = nextPoint.X - prevPoint.X;
            var dy = nextPoint.Y - prevPoint.Y;
            return Math.Sqrt(dx * dx + dy * dy);
        }

        public static void MakeOrders(Point[] checkpoints, int[] order, int position, double distance, int[] bestOrder)
        {
            double minDistance = int.MaxValue;
            if (position == order.Length)
            {
                if (distance < minDistance)
                {
                    minDistance = distance;
                    distance = 0;
                    for (int i = 0; i < bestOrder.Length; i++)
                        bestOrder[i] = order[i];
                }
                return;
            }

            for (int i = 1; i < order.Length; i++)
            {
                var index = Array.IndexOf(order, i, 0, position);
                if (index != -1)
                    continue;
                order[position] = i;
                distance += GetDistance(checkpoints[i - 1], checkpoints[i]);
                if (distance > minDistance)
                    return;
                MakeOrders(checkpoints, order, position + 1, distance, bestOrder);
            }
        }

		public static int[] FindBestCheckpointsOrder(Point[] checkpoints)
		{
			var bestOrder = new int[checkpoints.Length];
            var order = new int[checkpoints.Length];
            MakeOrders(checkpoints, order, 1, 0, bestOrder);
			return bestOrder;
		}

		private static int[] MakeTrivialPermutation(int size)
		{
			var bestOrder = new int[size];
			for (int i = 0; i < bestOrder.Length; i++)
				bestOrder[i] = i;
			return bestOrder;
		}
	}
}
