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

int partition(int A[], int left, int right, int x)
{
    int i;
    for (i = left; i < right; i++)

        if (A[i] == x)

            break;

    swap(&A[i], &A[right]);
    i = left;
    for (int j = left; j <= right - 1; j++)
    {
        if (A[j] <= x)
        {
            swap(&A[i], &A[j]);
            i++;
        }
    }

    swap(&A[i], &A[right]);

    return i;
}

int findMedian(int A[], int size)
{

    sort(A, A + size);

    return A[size / 2];
}

int kthSmallest(int A[], int left, int right, int k)
{
    if (k > 0 && k <= right - left + 1)
    {
        int size = right - left + 1;
        int i, median[(size + 4) / 5];

        for (i = 0; i < size / 5; i++)

            median[i] = findMedian(A + left + i * 5, 5);

        if (i * 5 < size)
        {
            median[i] = findMedian(A + left + i * 5, size % 5);
            i++;
        }
        int medOfMed = (i == 1) ? median[i - 1] : kthSmallest(median, 0, i - 1, i / 2);
        kthSmallest(median, 0, i - 1, i / 2);
        int pos = partition(A, left, right, medOfMed);

        if (pos - left == k - 1)

            return A[pos];

        if (pos - left > k - 1)
            return kthSmallest(A, left, pos - 1, k);
        return kthSmallest(A, pos + 1, right, k - pos + left - 1);
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
    int size = 10;
    int A[size];

    for (int i = 0; i < size; i++)
    {
        A[i] = (rand() % 1000 + 1);
    }

    auto start = std::chrono::high_resolution_clock::now();

    for (int i = 0; i < size; i++)
    {
        cout << A[i] << " ";
    }
    cout << endl;

    quickSort(A, 0, size - 1, size);

    for (int i = 0; i < size; i++)
    {
        cout << A[i] << " ";
    }
    cout << endl;

    auto finish = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> elapsed = finish - start;

    cout << elapsed.count();

    return 0;
}
