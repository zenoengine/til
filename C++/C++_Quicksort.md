# C++ Quick Sort

블로그에 예전에 올린 코드가 문제가 있어서 수정을 했습니다.

```cpp
#include <iostream>

using namespace std;

int partition(int* a, int low, int high)
{
    int pivotIdx = (low + high) / 2;

    int l = low;
    int h = high;

    while (l < h)
    {
        while(a[l] < a[pivotIdx] && l < h)
        {
            l++;
        }

        while(a[h] >= a[pivotIdx] && l < h)
        {
            h--;
        }

        if (l<h)
        {
            if (l == pivotIdx)
            {
                pivotIdx = h;
            }

            swap(a[l], a[h]);
        }
    }

    swap(a[pivotIdx], a[h]);
    
    pivotIdx = h;

    return pivotIdx;
}

void quicksort(int* a, int low, int high)
{
    if (low < high)
    {
        int pivotIdx = partition(a, low, high);

        quicksort(a, low, pivotIdx-1);
        quicksort(a, pivotIdx+1, high);
    }
}

int main()
{
    int n = 0;
    cin >> n;

    int* numbers = new int[n];

    srand(1);
    for (int i = 0; i < n; i++)
    {
        numbers[i] = rand()%100;
    }

    cout << "before : ";
    for (int i = 0; i < n; i++)
    {
        cout << numbers[i] << " ";
    }
    cout << endl;

    quicksort(numbers, 0, n - 1);

    cout << "after : ";
    for (int i = 0; i < n; i++)
    {
        cout << numbers[i] << " ";
    }
    cout << endl;

    cout << count;
    delete[] numbers;

    return 0;
}
```