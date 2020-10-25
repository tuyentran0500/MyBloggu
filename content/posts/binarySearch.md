---
title: "Tìm kiếm nhị phân"
date: 2020-10-20T01:13:40+09:00
draft: false
---
## Giới thiệu  
Bài toán tìm kiếm nhị phân là 1 bài toán cơ bản trong Computer Programming, nó giúp bạn tìm kiếm kết quả bài toán trong O(log(n)) thay vì phải duyệt tìm kiếm trong O(n). 
Trong bài viết này mình sẽ trình bày những ý tưởng cơ bản nhất của tìm kiếm nhị phân(Binary Search).  

## Bài toán 1  
> Cho 1 dãy tăng dần `a[1]...a[n]`, cho 1 số x, bạn cần in ra 2 kq:  
1. Phần tử đầu tiên trong dãy lớn hơn hoặc bằng x.  
2. Phần tử cuối cùng nhỏ hơn hoặc bằng x.  

**Lời giải**  
- Bài toán khá cơ bản, bạn có thể dùng 2 hàm `upper_bound` và `lower_bound` để giải quyết bài toán:  
1. `a[lower_bound(a+1,a+1+n,x) - a]`  
2. `a[upper_bound(a+1,a+1+n,x) - a -1]`  

Bạn có thể tìm hiểu về 2 hàm trên trong C++ ở đây:  
+ [lower_bound](http://www.cplusplus.com/reference/algorithm/lower_bound/)  
+ [upper_bound](http://www.cplusplus.com/reference/algorithm/upper_bound/?kw=upper_bound)  
Độ phức tạp của thuật toán là `O(log(n))` cho thao tác tìm kiếm.  

## [Bài toán 2](https://codeforces.com/gym/101021/problem/1)    
> Cho 1 số x ($1 \leq x \leq 10^6$) và bạn có tối đa 25 câu hỏi.  
Mỗi câu hỏi bạn sẽ hỏi 1 số, máy tính sẽ được ra kết quá `<` hoặc `>=` trả lời quan hệ của số x đối với số bạn đã hỏi.  
Sau khi hỏi xong tất cả câu hỏi, bạn cần đưa ra đáp án.  

**Lời giải**  
Đây là bài toán rất cơ bản về ý tưởng của chia nhị phân, nó rất nổi tiếng nên mình chỉ diễn tả ý tưởng cơ bản nhất.  
Giả sử đoạn tìm kiếm của bạn là [l,r], bạn cần hỏi mid = (l+r)/2.  
Nếu máy tính đưa ra kết quả: `<`, tức là số x sẽ nằm trong khoảng từ [l,mid-1], do đó bạn gắn `r = mid - 1`.  
Ngược lại, x sẽ nằm trong khoảng [mid,r], bạn gắn `l = mid`.  
Với mỗi thao tác, bạn sẽ thu hẹp được 1 nửa số phần tử bạn cần tìm kiếm, số thao tác bạn cần hỏi sẽ là `log[n]`, với n là độ dài của khoảng bạn cần tìm kiếm.  
VD: Khoảng tìm kiếm ban đầu của bạn là [1, 10] số bạn cần tìm là 6, thì trình tự hỏi của bạn sẽ là:  

|Câu hỏi | Máy trả lời | Khoảng tìm kiếm|
|:------:|:-----------:|:--------------:|
|5       | >=          | [5...10]       |
|7       | <           | [5...6]        |
|6       | >=          | [6]            |
Đến lúc này bạn có thể đoán được rồi, kết quả sẽ là 6.  
Một vài bài tập tương tự:  
+ [CF706B](https://codeforces.com/problemset/problem/706/B)    
+ [C11SEQ](https://vn.spoj.com/problems/C11SEQ/)  

## [Bài toán 3](https://vndoc.com/de-thi-chon-hoc-sinh-gioi-cap-thanh-pho-lop-9-mon-tin-hoc-so-gd-dt-ha-noi-nam-hoc-2018-2019/download?fbclid=IwAR30rbViTFgIuMFIxGrl4kUtGCBqDWu0ApuUVnwyGPrp88jniUhWaIcb10k)  
> [Bài 4] Cho 1 bảng A có kích thước n x m, tìm hình chữ nhật k x k sao cho giá trị nhỏ nhất của nó là lớn nhất.  
+ $1 \leq k \leq n \leq 1000$   
+ $1 \leq A_{i,j} \leq 10^6$  

**Nhận xét:** Trong các bài toán tìm kiếm nhị phân, bạn sẽ rất hay thấy pattern: `Tìm giá trị nhỏ nhất trong các giá trị lớn nhất`, hoăc `giá trị lớn nhất trong các giá trị nhỏ nhất`... Trong những trường hợp thế này, đa số bạn sẽ kết hợp chặt nhị phân kết quả và sử dụng các thuật toán phụ để kiếm tra kết quả bài toán(Tham lam, quy hoạch động, etc...)  

**Lời giải**  
Thay vì for từng hình chữ nhật được và tìm giá trị nhỏ nhất của từng bảng k x k bằng các thuật toán tịnh tiến(two pointers), hoặc các cấu trúc dữ liệu phức tạp, bạn hoàn toàn có thể chặt nhị phân kết quả và đưa ra 1 giải thuật với độ phức tạp tương đối ổn.  

Gọi hàm `bool f(v)`, với `f(v) = 1` khi và chỉ khi tồn tại 1 bảng k x k mà ở đó tất cả giá trị trong bảng đều lớn hơn hoặc bằng v.  
Bạn có thể thấy rằng hàm này có giá trị giảm dần, tức là nó có **tính đơn điệu**.    
Do đó chúng ta có thể tiến hành chia nhị phân kết quả.  
Vậy, làm thế nào để tính hàm `f(v)` một cách hiệu quả?  
Tạo bảng B có kích tướng n x m, và `B[i][j] = (A[i][j] >= v)`. Bây giờ bài toán sẽ được chuyển đổi thành tìm 1 một hình vuông con của B có kích thước k x k và có toàn giá trị 1, đây là 1 bài toán có thể giải quyết hiệu quả bằng mảng dồn.  
Source code:  
```
/*input
5 2
1 11 2 3 3
9 9 2 3 3
2 2 2 2 2
1 2 2 5 6
4 10 2 7 8
*/
#include <bits/stdc++.h>
#define mid ((l+r)/2)

using namespace std;
const int N = 1005;
int a[N][N], n, k;
int b[N][N];
int get(int x1,int y1,int x2,int y2){
  return b[x2][y2] - b[x1-1][y2] - b[x2][y1-1] + b[x1-1][y1-1];
}
bool f(int minVal){
  for(int i=1;i<=n;++i){
    for(int j=1;j<=n;++j){
      b[i][j] = (a[i][j] >= minVal);
      b[i][j] += b[i-1][j] + b[i][j-1] - b[i-1][j-1];
    }
  }
  for(int i=1;i+k-1<=n;++i) for(int j=1;j+k-1<=n;++j) 
    if (get(i,j,i+k-1, j+k-1) == k*k) return 1;
  return false;
}
int main(){
  ios_base::sync_with_stdio(0); cin.tie(0);
  cin >> n >> k;
  for(int i=1;i<=n;++i){
    for(int j=1;j<=n;++j){
      cin >> a[i][j];
    }
  }
  int l = 1, r = 100000;
  int ans = 0;
  while (l <= r){
    if (f(mid)) {
      ans = mid;
      l = mid+1;
    }
    else {
      r = mid - 1;
    }
  }
  cout << ans;
}
```  
Một số bài toán tương tự.  
+ [MTWALK](https://codeforces.com/group/FLVn1Sc504/contest/274509/problem/Y) (level 1)      
+ [PRESENT](https://codeforces.com/problemset/problem/460/C) (Level 1)  
+ [Odd-Even Subsequence](https://codeforces.com/problemset/problem/1370/D) (Level 1)    
+ [Socket](https://codeforces.com/problemset/gymProblem/100886/J)  (Level 1)
+ [Friend and Subsequence](https://codeforces.com/problemset/problem/689/D)  (Level 2)  

## [Bài toán 4](https://vn.spoj.com/problems/EGG/)  
> Cho tòa nhà M tầng, bạn có N quả trúng. Độ cứng của N quả này là như nhau, Độ cứng của quả trứng là E nếu ta thả từ tầng 1..E quả trứng không vỡ, nhưng từ tầng E+1 thì quả trứng vỡ. Đếm số lần ít nhất để xác định được độ cứng của quả trứng.  

**Lời giải**  
Gọi f[i][j] là số lần thả trứng ít nhất để xác định độ cứng nếu tòa nhà ở tầng thứ i và có j lần thả.  
    + Nếu ta thả ở tầng cuối cùng, `f[i][j] = f[i-1][j-1] +1`.  
	+ Nếu ta thả ở tầng thứ z( z $\leq$ i). Thì bài toán chia thành 2 phần: `max(f[i-z][j-1], f[z][j-1])`.  
    Dễ thấy f[i-z][j-1] giảm dần khi z tăng, và f[z][j-1] lại tăng.  
Do đó, max(f[i-z][j-1], f[z][j-1]) min khi f[i-z][j-1] nhỏ nhất và f[z][j-1] lớn nhất mà `f[i-z][j-1] >= f[z][i-1]`.  
Nhờ đó ta có 1 cách tiếp cận bằng phương pháp tìm kiếm nhị phân.  
```
#include <stdio.h>
#include <algorithm>
#define For(i,a,b) for(int i=(a);i<=(b);++i)
using namespace std;
const int N = 1001;
int n,m,t,f[N][N];
#define mid ((l+r)/2)
int main(){
	For(i,1,N-1) 	f[1][i] = 1, f[i][1] =  i;
	For(i,2,N-1){
		For(j,2,N-1){
			f[i][j] = min(f[i-1][j], f[i-1][j-1]) + 1;
			int l = 2 , r = i-1;
			//f[i][j] = min(f[i][j], max(f[z-1][j-1], f[i-z][j]) + 1);  
			while (l!=mid && mid!=r){
				if (f[mid-1][j-1] >= f[i-mid][j]) r = mid;
				else l = mid;
			}
			For(z,l,r) f[i][j] = min(f[i][j], max(f[z-1][j-1], f[i-z][j]) + 1);  
		}
	}
	scanf("%d", &t);
	while (t--){
		scanf("%d%d", &n, &m);
		printf("%d\n", f[m][n]);
	}
}
```  
Bài toán tương tự:  
- [Levko and Array](https://codeforces.com/contest/360/problem/B)  

   










