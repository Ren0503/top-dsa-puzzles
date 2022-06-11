- Mức độ: Khó
- Hỏi bởi: Google, Amazon, Samsung

## Hiểu vấn đề

Cho một mảng bao gồm cả số dương và số âm, viết một chương trình tìm số nguyên dương đầu tiên còn thiếu.

**Ví dụ 1**
- Input: X[] = [2, -9, 5, 11, 1, -10, 7]
- Output: 3
- Giải thích: Trong mảng hiện tại bao gồm hai số dương là 1 và 2. Thế nên số dương còn thiếu tiếp là 3.

**Ví dụ 2**
- Input: X[] = [2, 3, -2, 1]
- Output: 4
- Giải thích: Trong mảng hiện tại có cả 1, 2 và 3. Nên số dương đầu tiên còn thiếu là 4.

**Ví dụ 3**
- Input: X[] = [4, 6, 5, 3, 8]
- Output: 1
- Giải thích: Số dương đầu tiên là 1 hiện không có trong mảng, nên output là 1.

## Thảo luận giải pháp

- Brute force sử dụng 2 vòng lặp lồng nhau.
- Sắp xếp và quét.
- Dùng bảng băm.
- Dùng phép băm tại chỗ.
- Kết hợp phân tách và băm tại chỗ.

## Cách tiếp cận brute force dùng hai vòng lặp

### Ý tưởng

Nếu số dương còn thiếu là k, thì tất cả các vị trí từ 1 đến k-1 phải có trng mảng. Thế nên câu hỏi cốt lõi là: Số có giá trị lớn nhất có thể bị thiếu trong một mảng với kích thước n là bao nhiêu? Câu trả lời đơn giản là: với một mảng kích cỡ n, số có giá trị lớn nhất có thể bị thiếu là n + 1, trong trường hợp này, tất cả các số từ 1 đến n đã xuất hiện trong mảng.

Giải pháp cơ bản cho vấn đề này là tìm tất cả các số từ 1 đến n + 1. Nếu bất kỳ số nguyên nào bị thiếu ta sẽ trả về giá trị đó.

```cpp
int firstMissingPositive(int X[], int n) {
    for (int i = 1; i <= n+1; i++) {
        bool missingFlag = true;
        for (int j = 0; j < n; j++) {
            if (X[j] == i) {
                missingFlag = false;
                break;
            }
        }
        if (missingFlag == true) 
            return i;
    }
}
```

### Phân tích giải pháp

Ta đang chạy hai vòng lặp thực hiện một thao tác hằng số ở mỗi vòng lặp. Ở đây cả hai vòng lặp đều chạy n lần trong trường hợp tệ nhất. Thế nên độ phức tạp thời gian trong trường hợp tệ nhất = `O(n)*O(n)*O(1)` = `O(n^2)`. Ta đang sử dụng không gian bổ sung là hằng số nên độ phức tạp không gian là O(1).

## Giải pháp sắp xếp và một lần quét

### Ý tưởng

Nếu ta sắp xếp lại mảng, tất cả vị trí số dương sẽ theo trật tự tăng dần, và ta có thể tìm kiếm tuyến tính để tìm số dương đầu tiên còn thiếu. Để sắp xếp, ta có thể sử dụng thuật toán sắp xếp như heapsort, mergesort và quicksort.
1. Ta sắp xếp lại mảng. Ta có thể sử dụng heapsort, thuật toán sắp xếp có độ phức tạp thời gian là `O(nlogn)`.
2. Bây giờ ta chạy một vòng lặp để bỏ qua các số âm và số 0 trong mảng. Bắt đầu từ chỉ mục đầu tiên, khi ta tìm thấy `X[i] < 1`, ta tăng `i` lên 1 đơn vị. Đến khi vòng lặp kết thúc `i` sẽ biểu diễn giá trị dương đầu tiên trong mảng.
3. Bây giờ ta chạy một vòng lặp khác j = i đến n - 1 để tìm số dương đầu tiên bị thiếu. Bên ngoài vòng lặp ta khai báo biến `missingPositive` để theo dõi giá số dương bị thiếu. `missingPositive` sẽ được khởi tạo với giá trị dương nhỏ nhất là 1 khi bắt đầu.
4. Ta bắt đầu so sánh `missingPositive` với `X[i]`. Nếu chúng bằng nhau, ta chuyển đến phần tử kế tiếp và tăng `missingPositive` lên 1.
5. Ta tiếp tục thực hiện cho đến khi tìm thấy giá trị `X[i] > missingPositive`. Tại giai đoạn này, giá trị được lưu lại ở `missingPositive` sẽ là số dương đầu tiên bị thiếu.

### Mã giả

```cpp
int firstMissingPositive(int X[], int n) {
    heapSort(X, n);
    int i = ;
    while (X[i] < 1)
        i++;
    missingPositive = 1;
    for (int j = 1; j < n; j++) {
        if (missingPositive == X[i])
            missingPositive = missingPositive + 1;
        else if (X[i] > missingPositive)
            return missingPositive;
    }
    return missingPositive;
}
```

### Phân tích giải pháp

-  Độ phức tạp thời gian = Độ phức tạp thời gian của thuật toán + Quét tìm kiếm số dương đầu tiên bị thiếu = `O(nlogn) + O(n)` = O(nlogn).
- Độ phức tạp không gian = O(1)

## Sử dụng bảng băm

### Ý tưởng

Bây giờ ta sẽ tập trung vào cải thiện độ phức tạp thời gian của giải pháp. Như đã nhấn mạnh ở giải pháp brute force, ta tìm kiếm tất cả số dương từ 1 đến n+1 rồi xem số nào thiếu ở trong mảng. Vậy liệu có cách nào tối ưu hoá hơn không..

Để tìm kiếm hiệu quả hơn, một ý tưởng ở đây là thay vì tìm kiếm tuyến tính ta sử dụng một bảng băm. Bảng băm có thể giúp ta tìm kiếm và chèn hiệu quả hơn với thời gian trung bình là O(1).

**Các bước**
- Ta tạo một bảng băm H với kích thước n.
- Bây giờ chạy một vòng lặp từ i=0 đến n-1 và thêm tất cả phần tử vào bảng băm H.
- Ta thực hiện một vòng lặp khác, từ i=1 đến n+1 để tìm kiếm tất cả số nguyên trong bảng băm H có trong mảng không. Nếu tại chỉ mục `i` không có trong H ta trả về `i`. Ngược lại ta đến vòng lặp kế tiếp.

### Mã giả

```cpp
int firstMissingPositive(int X[], int n) {
    HashTable H;
    for (int i = 0; i < n; i++)
        H[X[i]] = true;
    for (int i = 1; i <= n+1; i++) {
        if (H[i] == false)
            return i;
    }
}
```

### Phân tích giải pháp

- Độ phức tạp thời gian = Thêm n phần tử vào bảng băm + Tìm n phần tử trong bảng băm = O(n) + O(n) = O(n).
- Độ phức tạp không gian = O(n) do sử dụng bảng băm có kích thước n.

## Giải pháp băm tại chỗ

### Ý tưởng

Bây giờ ta đi sâu vào câu hỏi: liệu ta có thể giải quyết vấn đề này in-place không (in-place tức chỉnh sửa trên chính mảng đầu vào không dùng mảng phụ).
- Ta cần xác định số dương đầu tiên bị thiếu trong khoảng từ 1 đến n+1. Để thực hiện, ta có thể sử dụng các chỉ mục của mảng để đánh đấu sự hiện diện của các số trong khoảng trên và đặt từng số vào đúng vị trí của nó `X[X[i]-1] == X[i]`. Ví dụ ta đặt 1 vào X[0], 2 vào X[1], và 3 vào X[2].
- Để đặt các số vào đúng vị trí, ta có thể quét mảng X[] từ đầu và kiểm tra số X[i] có trong phạm vi từ 1 đến n không nếu có nó có nằm đúng chỉ mục `X[i] - 1` không. Nếu điều kiện đầu đúng và điều kiện hai sai, ta hoán đổi X[i] với số hiện tại đang ở chỉ mục `X[i] - 1`. Ngược lại, ta bỏ qua và nhảy đến chỉ mục tiếp theo.
 Bây giờ, ta quét lại mảng lần nữa để tìm xem X[i] != i + 1.  Nếu tồn tại X[i] != i +1 ta trả về i +1 là số dương đầu tiên còn thiếu của mảng, nếu không tìm tìm thấy bất kỳ chỉ mục nào, ta trả về n+1 là số dương đầu tiên còn thiếu. Điều này đồng nghĩa trong mảng có các phần tử từ 1 đến n.
 
 ### Mã giả
 
 ```cpp
 int firstMissingPositive(int X[], int n) {
     int i = 0;
     while (i < n) {
         if (X[i] > 0 && X[i] <= n && X[X[i] - 1] != X[i])
             swap(X[i], X[X[i] - 1]);
         else
              i++;
     }
     for (int i = 0; i < n; i++) {
          if (X[i] !=i+1)
              return i+1;
     }
     return n+1;
 }
 ```

### Phân tích giải pháp

- Độ phức tạp thời gian trong trường hợp tệ nhất = Độ phức tạp thời gian của chỉnh sửa mảng + Độ phức tạp thời gian của tìm số dương đầu tiên còn thiếu = O(n) + O(n) = O(n)
- Độ phức tạp không gian = O(1), vì ta chỉnh sửa trên chính mảng đầu vào nên không sử dụng thêm không gian bổ sung.

## Giải pháp phân tách và băm tại chỗ

### Ý tưởng

Ý tưởng này tương tự giải pháp ở trên, nhưng có một điểm khác. Trước tiên ta tách mảng thành hai phần, một phần toàn số dương và một phần toàn số âm.  Ta đang giả sử mảng có thể được chỉnh sửa:
- Ta chia mảng đầu vào thành các nhóm toàn số âm và toàn số dương bằng cách sử dụng thuật toán tương tự quicksort. Sau khi tách mảng số dương bắt đầu từ chỉ mục 0 đến **positiveEnd** -1 . Ở đây giá trị **positiveEnd** được trả về bởi quá trình phân tách
- Bây giờ ta duyệt mảng từ 0 đến positiveEnd - 1. Nếu X[i] > positiveEnd, ta không làm gì cả. Ngược lại ta đánh dấu các chỉ mục X[i] - 1 là âm.
- Cuối cùng, chúng ta duyệt mảng một lần nữa từ chỉ số 0 đến positiveEnd -1. Nếu chúng ta gặp một phần tử dương ở chỉ số `i` nào đó, ta xuất i + 1 là phần tử dương đầu tiên bị thiếu. Nếu chúng ta không gặp bất kỳ phần tử dương nào, thì các số nguyên từ 1 đến positiveEnd có trong mảng và chúng ta trả về positiveEnd + 1. *Lưu ý*: Cũng có thể xảy ra trường hợp tất cả các số đều không dương, làm cho positiveEnd = 0. Đầu ra positiveEnd + 1 = 1 vẫn đúng

**Ta có thể hiểu đơn giản hơn với ví dụ dưới đây**

Mảng đầu vào: [1, -1, -5, -3, 3, 4, 2, 8]

- **Bước 1:** Phân tách thành [1, 8, 2, 4, 3] và [-3, -5, -1] với positiveEnd = 5
- **Bước 2:** Chỉnh sửa mảng đầu vào
    - `1 < positiveEnd`, ta thay đổi dấu của giá trị tại chỉ mục [1-1] = 0
    Mảng được chỉnh sửa: [-1, 8, 2, 4, 3]
    - `8 > positiveEnd`, không thực hiện gì nhảy sang phần tử kế tiếp.
    - `2 < positiveEnd`, ta thay đổi dấu của giá trị tại chỉ mục [2-1] = 1
    Mảng được chỉnh sửa: [-1, -8, 2, 4, 3]
    - `4 < positiveEnd`, ta thay đổi dấu của giá trị tại chỉ mục [4-1] = 3
    Mảng được chỉnh sửa: [-1, -8, 2, -4, 3]
    - `3 < positiveEnd`, ta thay đổi dấu của giá trị tại chỉ mục [3-1] = 2
    Mảng được chỉnh sửa: [-1, -8, -2, -4, 3]
- **Bước 3:** Duyệt từ chỉ mục 0 đến positiveEnd, ta tìm thấy X[4] = 3 > 0. Kết quả là 4 + 1 = 5

### Mã giả

```cpp
int firstMissingPositive(int X[], int n) {
    int positiveEnd = partition(X, n);
    for (int i = 0; i < positiveEnd; i++) {
        if (X[i] - 1 < positiveEnd)
            X[X[i] - 1] = - X[X[i] - 1];
    }
    for (int i = 0; i < positiveEnd; i++) {
        if (X[i] > 0)
            return i + 1;
    }
    return positiveEnd + 1;
}

int partition(int X[], int n) {
    int j = n - 1, i = 0;
    while (i <= j){
        if (A[i] <= 0) {
            swap(A[i], A[j]);
            j--;
        }
        else
            i++;
    }
    return i
}
```

### Phân tích giải pháp

- Độ phức tạp thời gian = Độ phức tạp thời gian của phân vùng + độ phức tạp thời gian của chỉnh sửa mảng + độ phức tạo thời gian của tìm số dương đầu tiên còn thiếu = O(n) + O(n) + O(n) = O(n).
- Độ phức tạp không gian = O(1)

## Suy nghĩ thêm

- Trong cách tiếp cận thứ 4 và thứ 5, tại sao chúng ta không lưu trữ số lần xuất hiện của các số lớn hơn n?
- Điều gì xảy ra nếu chúng ta cần tìm số âm lớn nhất còn thiếu?
- Có thể xuất tất cả các số bị thiếu trong mảng trong phạm vi [1, n] không?
- Trong cách tiếp cận thứ 2, mảng của chúng ta được sắp xếp. Chúng ta có thể nghĩ để tối ưu hóa việc tìm kiếm bằng cách áp dụng thuật toán tìm kiếm nhị phân không?
- Chúng ta có thể giải quyết vấn đề bằng cách sử dụng một số cách tiếp cận khác không?
- Cách tiếp cận thứ 4 và thứ 5 sẽ hoạt động nếu mảng đầu vào chứa các số lớn?
- Trong cách tiếp cận thứ 3, chúng ta có cần lưu trữ tất cả các số nguyên không dương không?
- Điều gì sẽ xảy ra nếu tất cả các phần tử có trong mảng là âm? Các thuật toán trên có bao gồm điều kiện này không?