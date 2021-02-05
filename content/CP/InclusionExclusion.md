---
title: "Bao hàm và loại trừ"
date: 2020-11-01T16:23:25+09:00
draft: false
---  
## Giới thiệu  
Bao hàm và loại trừ (Inclusion and Exclusion) là 1 khái niệm rất cơ bản trong toán đếm và tổ hợp, nó cũng có ứng dụng trong việc học CP. Trong bài viết này, mình sẽ viết lại 1 số bài tập và ứng dụng mà mình học được trong 2 tuần học về bài này.  
Bài viết này có tham khỏa từ [VNOI](https://vnoi.info/wiki/translate/he/Number-Theory-7.md)  

## Công thức.  
Phát biểu: 
> "Để tính lực lượng của hợp của nhiều tập hợp, ta tính tổng lực lượng các tập hợp đó, rồi trừ đi lực lượng của giao của các cặp hai tập hợp khác nhau, rồi cộng lực lượng của giao các bộ ba tập hợp khác nhau, rồi trừ đi lực lượng của các bộ bốn tập hợp, và cứ thế cho đến khi ta xét đến giao của tất cả các tập hợp."  - VNOI Wiki.  

Chúng ta có thể dùng 1 ví dụ đơn giản: 1 lớp có 20 học sinh thích toán, 20 học sinh thích văn, 10 học sinh thích cả 2 môn. Hỏi lớp đó có bao nhiêu học sinh thích cả 2 môn.  
Gọi tập A là tập số học sinh thích toán, B là tập số học sinh thích văn, và $A \cap B$ là tập các học sinh thích cả 2 môn.  
Ta có số học sinh thích ít nhất 1 trong 2 môn là:  

$$|A \cup B| = |A| + |B| - |A \cap B|$$  

Dạng tổng quát:  

$$|\bigcup_{i=1}^{i=n} A_i| = \sum_{i=1}^{n} (-1)^{k+1} \sum_{1 \leq i_1 < i_2 ... \leq i_k} |A_{i_1} \cap ... \cap A_{i_k}|$$  

## [Bài toán 1: Giảm chiều quy hoạch động](https://atcoder.jp/contests/abc180/tasks/abc180_f)  
Bài toán:   
> Tìm số lượng đồ thị N đỉnh, M cạnh, không nhất thiết phải liên thông hoặc là đồ thị đơn, thỏa mãn các điều kiện sau:  
    - Không có `self-loop` (Tức là 1 cạnh nối từ 1 đỉnh đến chính nó).  
    - Bậc của của mỗi đỉnh tối đa là 2.  
    - Kích thước lớn nhất của 1 thành phần liên thông là L.  
    Giới hạn:  
    + $2 \leq N \leq 300$  
    + $1 \leq M \leq N$  
    + $1 \leq L \leq N$  

**Lời giải**    
Thay vì giải quyết bài toán là đếm trực tiếp có bao nhiều đồ thị mà ở đó thành phần liên thông lớn nhất có kích thước là L. Ta sẽ đếm xem có bao nhiều đồ thị mà thành phần liên thông có **kích thước tối đa** có thể là L trừ đi số thành phần liên thông có **kích thước tối đa** là L-1.  
Từ đó ta có công thước QHĐ: DP(i,j) là số cách để xây dựng đồ thị gồm i đỉnh j cạnh thỏa mãn cách độ lớn của thành phần liên thông không được vượt quá S.     
Bởi vì bậc của mỗi đỉnh tối đa là 2 nên 1 đỉnh u sẽ thuộc về 1 đường đi, hoặc 1 chu trình, hoặc nó sẽ tồn tại độc lập.  
Do đó, ta sẽ có công thức quy hoạch động sau: Với DP(i,j) ta có đỉnh x là đỉnh nhỏ nhất mà tập đồ thị thỏa mãn đề bài chưa để tối ưu DP(i,j). Từ đó, ta sẽ thêm đỉnh x vào tập i đỉnh j cạnh.  
- TH1: Nếu $1 \leq S$, ta có thể thêm đỉnh x vào như 1 thành phân liên thông độc lập: `dp[i+1][j] += dp[i][j]`.  
- TH2: Đỉnh x thuộc về 1 đường thẳng gồm k đỉnh ($k \leq S$). Số cách chọn k - 1 đỉnh còn lại là $C_{n - i - 1}^{k-1}$ và mỗi cách chọn đấy ta sẽ có $\frac{k!}{2}$ hoán vị đường thẳng. `dp[i+k][j + k - 1] += dp[i][j] * C[k-1][n-i-1] * k!/2`.  
- TH3: Tương tự, ta xét trường hợp x thuộc về 1 chu trình. Lưu ý ở đây ta sẽ chia thành 2 TH con:  
    + TH3.1: x tạo với 1 điểm khác thành 1 thành phần liên thông có kích thước bằng 2: `dp[i+2][j+2] += dp[i][j] * (n - i - 1)` (Với $S \geq 2$).  
    + TH3.2: x tạo với k-1 đỉnh khác thành 1 chu trình gồm k đỉnh: `dp[i+k][j+k] += dp[i][j] * C[k-1][n-i-1]*(k-1)!/2`.  

Code: [Atcoder](https://atcoder.jp/contests/abc180/tasks/abc180_f)  
**Nhận xét:** Đây không phải là một quan sát bao hàm và loại trừ thuần túy, tuy nhiên bản thân mình thấy nó lại rất có ích trong nhiều bài toán. Do đó mình đã quyết định thêm bài này vào.  
## [Bài toán 2: Đơn giản hóa bài toán](https://codeforces.com/problemset/problem/1228/E)  
> Cho bảng n x n và số k. Đặt vào mỗi ô trong bảng các số từ 1 đến k sao cho giá trị nhỏ nhất của mọi hàng và mọi cột đều là 1. Đếm số cách có thể.  
> Giới hạn: $1 \leq n \leq 250, 1 \leq k \leq 10^9$  

Với bài toán này, ta hoàn toàn có thể áp dụng bao hàm và loại trừ 1 cách trực tiếp: Gọi ans[i][j] là số cách chọn mà có ít nhất i hàng có giá trị nhỏ nhất lớn hơn 1, và j cột có giá trị nhỏ nhất lớn hơn 1. Toàn bộ `(i+j)*n - i*j` ô này sẽ là được gắn cách giá trị từ 2...k, các ô còn lại sẽ được gắn với giá trị từ 1...k.  
Do đó, ta có công thức:  

$$Answer = \sum_{1 \leq i, j \leq n}(-1)^{i+j} ans(i,j) = \sum_{1 \leq i, j \leq n} k^{(n-i-j)*n + i*j}(k-1)^{(i+j)*n - i * j}$$

Code: [Codeforces](https://codeforces.com/contest/1228/submission/96328859)  
## [Bài toán 3: Đếm các số nguyên tố cùng nhau](https://codeforces.com/contest/920/problem/G)  
> Cho t($t \leq 30000$) truy vấn mỗi truy vấn gồm 3 số x, p, k (1 \leq x,p,k \leq 10^6$). Bài toán đưa ra là tìm số thứ k trong tập L(x, p) với L(x, p) là tập các y > x mà nguyên tố cùng nhau với p. VD: L(7, 22) = 9, 13, 15,...  

**Lời giải**  
Đây là 1 bài toán kinh điển, với điều kiện cần tìm số thứ k của 1 dãy nào đấy, ý tưởng của ta là sử dụng tìm kiếp nhị phân để đếm xem trong tập từ 1...y có bao nhiêu số nguyên tố cùng nhau với p.  
Từ đó, ta có hàm chia nhị phân tương tự như sau: 
```
	while (l <= r){
		if (get(mid,p) - get(x,p) >= k){
			ans = mid;
			r = mid - 1;
		}
		else l = mid + 1;
	}
```  
Với get(y, p): là số các số trong tập 1...y nguyên tố cùng nhau với p.  
Vậy làm thế nào để tính hàm get(y, p)?  
Ta có thể phân tích các ước của số p và đếm xem có bao nhiêu số trong tập từ 1...y chia hết cho số đó.  
Từ đó ta có công thức bao hàm và loại trừ.  
Code: [Codeforces](https://codeforces.com/contest/920/submission/96772908)  
**Bài toán tương tự**: [Mike and Foam](https://codeforces.com/problemset/problem/547/C)  













