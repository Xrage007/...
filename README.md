#include <iostream>
#include <chrono>
#include <algorithm>

using namespace std;

void swap(int *a, int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int arr[], int left, int right, int x)
{
    int i;
    for (i = left; i < right; i++)

        if (arr[i] == x)

            break;

    swap(&arr[i], &arr[right]);
    i = left;
    for (int j = left; j <= right - 1; j++)
    {
        if (arr[j] <= x)
        {
            swap(&arr[i], &arr[j]);
            i++;
        }
    }

    swap(&arr[i], &arr[right]);

    return i;
}

int findMedian(int arr[], int n)
{

    sort(arr, arr + n);

    return arr[n / 2];
}

int kthSmallest(int arr[], int left, int right, int k)
{
    if (k > 0 && k <= right - left + 1)
    {
        int n = right - left + 1;
        int i, median[(n + 4) / 5];

        for (i = 0; i < n / 5; i++)

            median[i] = findMedian(arr + left + i * 5, 5);

        if (i * 5 < n)
        {
            median[i] = findMedian(arr + left + i * 5, n % 5);
            i++;
        }
        int medOfMed = (i == 1) ? median[i - 1] : kthSmallest(median, 0, i - 1, i / 2);
        kthSmallest(median, 0, i - 1, i / 2);
        int pos = partition(arr, left, right, medOfMed);

        if (pos - left == k - 1)

            return arr[pos];

        if (pos - left > k - 1)
            return kthSmallest(arr, left, pos - 1, k);
        return kthSmallest(arr, pos + 1, right, k - pos + left - 1);
    }
    return -1;
}

void quickSort(int A[], int left, int right, int size)
{
    if (left < right)
    {
        cout << "Left: " << left << "  "
             << "Right: " << right << endl;

        cout << endl;
        int piv = kthSmallest(A, left, right, int(size / 2));

        quickSort(A, left, piv - 1, size);
        quickSort(A, piv + 1, right, size);
    }
}

int main()
{
    int n = 10;
    int A[n];

    for (int i = 0; i < n; i++)
    {
        A[i] = (rand() % 1000 + 1);
    }

    auto start = std::chrono::high_resolution_clock::now();

    for (int i = 0; i < n; i++)
    {
        cout << A[i] << " ";
    }
    cout << endl;

    quickSort(A, 0, n - 1, n);

    for (int i = 0; i < n; i++)
    {
        cout << A[i] << " ";
    }
    cout << endl;

    auto finish = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> elapsed = finish - start;

    cout << elapsed.count();

    return 0;
}
