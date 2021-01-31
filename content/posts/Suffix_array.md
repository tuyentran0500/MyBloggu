---
title: "Suffix array - Mảng hậu tố"
date: 2021-01-24T10:45:25+09:00
draft: false
---  
# Giới thiệu  
Mảng hậu tố(suffix array) là 1 kĩ thuật xử lý xâu rất mạnh được áp dụng trong nhiều trường hợp. Trong bài viết này mình sẽ miêu tả về cách hoạt động của Suffix Array và 1 vài ứng dụng của nó.  
Bài viết này tham khảo từ 2 nguồn chính:  
- [CP algorithm](https://cp-algorithms.com/string/suffix-array.html)  
- [VNOI wiki](https://vnoi.info/wiki/algo/data-structures/suffix-array.md)  
# I. Lý thuyết
## [Bài toán 1: Mảng hậu tố](https://www.spoj.com/problems/SARRAY/)  
> Cho 1 xâu S, in ra mảng hậu tố của xâu đó ($|S| \leq 100000$)  

**Khái niệm:**
- Xâu S[i..N-1] là 1 xâu hậu tố nếu nó bắt đầu từ 1 vị trí bất kì i và kết thúc tại vị trí cuối cùng của xâu S. VD: S = [abcca] thì các xâu ["abcca", "bcca", "cca", "ca", "a"] là các xâu hậu tố.  
- Nhiệm vụ của bài toán là sắp xếp các xâu hậu tố này theo thứ tự từ điển.  và in ra 1 mảng với giá trị của mỗi phần tử là vị trí bắt đầu của xâu hậu tố đó.  
Trong VD đề bài, sau khi sắp xếp các xâu hậu tố, ta được:  

| Vị trí bắt đầu | Xâu hậu tố |  
| --- | --- |  
| 4 | a |  
| 0 | abcca |  
| 1 | bbca |  
| 3 | ca |  
| 2 | cca |  

Từ đó, ta được mảng hậu tố: [4, 0, 1, 3, 2]  

### **Lời giải**  
Trong mục này, mình sẽ chỉ nêu ra thuật toán O(Nlog(N)^2) vì đây là thuật toán đủ nhanh để pass và cũng có cách code tương đối nhẹ nhàng.  
Nếu các bạn có nhu cầu tìm hiểu thuật toán O(nlog(n)) bạn có thể tìm hiểu trên link [CP algorithm](https://cp-algorithms.com/string/suffix-array.html).  
**Ý tưởng:** Ta sắp xếp các xâu con liên tiếp có độ dài $2^i$, và dựa vào đó để so sánh các xâu có độ dài $2^{i+1}$.  
Hàm so sánh:  
```
bool cmpSA(int u, int v){
	if (rank[u] != rank[v]) return rank[u] < rank[v];
	u += gap; 
	v += gap;
	if (u < n && v < n) return rank[u] < rank[v];
	return u > v;
}
```  
Với rank[u] là thứ tự sắp xếp của xâu con [u, u + gap - 1].  
Như thấy ở hàm trên, ta muốn so sánh 2 xâu `[u, u + 2*gap - 1]` và xâu `[v, v + 2*gap - 1]`, ta so sánh 2 xâu [u, u+gap-1] và xâu [v, v+gap-1]. Nếu 2 xâu này bằng nhau, ta sẽ tiếp tục so sánh 2 xâu: `[u + gap, u + 2*gap - 1]` và xâu `[v + gap, v + 2*gap - 1]`.  

Build Suffix Array:  
```
int sa[N], rank[N], trank[N];
int gap,n;
void buildSA(){
	n = s.size();
	for(int i=0;i<n;++i) sa[i] = i, rank[i] = s[i];
	for(gap=1;;gap*=2){
		sort(sa,sa+n,cmpSA);
		for(int i=0;i<n-1;++i) trank[i+1] = trank[i] + cmpSA(sa[i], sa[i+1]); 
		for(int i=0;i<n;++i) rank[sa[i]] = trank[i]; // cập nhật rank[u] cho `gap` mới
		if (trank[n-1] == n-1) break; // đã hoàn thành việc sắp xếp.  
	}
}
```  

## [Bài toán 2: Dãy con tiền tố dài nhất của 2 xâu con](https://www.spoj.com/problems/LCS/)  
> Cho 2 xâu, tìm độ dài xâu con chung liên tiếp dài nhất của 2 xâu đó.  

**Nhận xét**  
Bài toán này tương đương với việc tìm dãy con liên tiếp tiền tố dài nhất(LCP - Longest Common Prefix) của 2 hậu tố bất kì.  
Ta có thể nhận thấy rằng kết quả bài toàn sẽ là **max(độ dài xâu con chung dài nhất của 2 xâu liên tiếp trong mảng hậu tố)**.  
Do đó, bây giờ nhiệm vụ của chúng ta là tìm LCP của 2 xâu liền kề trong mảng hậu tố.    
Ta có thể sử dụng RMQ để tìm LCP của 2 xâu hậu tố trong O(log(n)). Tổng độ phức tạp của thuật toán sẽ là O(nlog(n)). Chi tiết bạn có thể đọc bài viết [CP Algorithm](https://cp-algorithms.com/string/suffix-array.html)  
Tuy nhiên, ta hoàn toàn có thể giảm độ phức tạp của thuật toán xuống **O(n)**, và mình sẽ tập trung vào lời giải này.  
### **Lời giải**  
Ta sẽ dùng thuật toán Kansal để tìm LCP.   

Để tiện, mình định nghĩa 1 vài khái niệm sau:  
- suffix[i] là xâu hậu tố bắt đầu từ vị trí i.  
- rank[i] là vị trí xâu hậu tố suffix[i] trong mảng hậu tố.  
- sa[i] tương đương với xâu hậu tố trong mảng hậu tố.  
VD với xâu: S = "abcca"  
Thì sa[0] = "a", sa[1] = "abcca", ... sa[4] = "cca".  
| Vị trí bắt đầu | Xâu hậu tố |  
| --- | --- |  
| 4 | a |  
| 0 | abcca |  
| 1 | bbca |  
| 3 | ca |  
| 2 | cca |  

- LCP(a, b) = độ dài của dài con chung tiền tố dài nhất của 2 xâu hậu tố a và b.  

**Nhận xét 1:** Cho 2 xâu hậu tố sa[i] và sa[j], thì `LCP(sa[i], sa[j]) = min(lcp[i], lcp[i+1], ... lcp[j-1])`. Với `lcp[i] = LCP(sa[i], sa[i+1])`.  
**Nhận xét 2:** Gọi vị trí bắt đầu của 2 xâu `sa[x] = i` và `sa[x+1] = j`. và `LCP(sa[x], sa[x+1]) = k`. Thì ta biết được rằng `LCP(sa[rank[i+1]], sa[rank[i+1] + 1]) >= k - 1`. Dựa vào nhận xét này, ta sẽ có thuật toán 2 con trỏ cho việc xây bảng LCP.  
```
    void buildLCP()
    {
        for (int i = 0, k = 0; i < N; ++i) if (rank[i] != N - 1)
        {
            for (int j = sa[rank[i] + 1]; S[i + k] == S[j + k];)
            ++k;
            lcp[rank[i]] = k;
            if (k)--k;
        }
    }
```  
Ta hoàn toàn có thể chứng minh **nhận xét 2** dựa trên **nhận xét 1**.  
Note: suffix[i] là xâu hậu tố bắt đầu từ vị trí i.
Do `lcp[x] = LCP(sa[x], sa[x+1]) = LCP(suffix[i], suffix[j]) = k` nên `LCP(suffix[i+1], suffix[j+1]) = k - 1`.  
Mà bởi vì xâu `suffix[i] = sa[x], suffix[j] = sa[x+1]`, nên `suffix[i] < suffix[j]`. Do đó, `suffix[i+1] < suffix[j+1]`.  
Do đó `LCP(suffix[i+1], suffix[j+1]) = min(lcp[rank[i+1]], lcp[rank[i+1] + 1], ..., lcp[rank[j] - 1]) >= k - 1`  
Từ đó ta biết được rằng `lcp[rank[i+1]] >= k - 1`.  
Bởi vậy ta sẽ có 1 thuật toán như trên, for các vị trí i trong xâu S, tìm lcp của xâu suffix[i] và xâu liền kề nó, cập nhật k và trừ giá trị k đi 1 đơn vị cho lần cập nhật của vị trí i+1.  
## [Bài toán 3: Đếm số substring khác nhau](https://codeforces.com/edu/course/2/lesson/2/5/practice/contest/269656/problem/A)  
> Cho 1 xâu s $(1 \leq s \leq 10^5)$. Đếm số substrings - xâu con liên tiếp phân biệt của xâu s.  

Bài toán này là 1 bài toán cơ bản, xuất hiện trong ứng dụng của thuật toán KMP, tuy nhiên độ phức tạp của thuật toán trên lại là $O(N^2)$. Tuy nhiên, Suffix Array đã cung cấp 1 lời giải khá đơn giản và có độ phức tạp $O(N)$ (không tính quá trình xây bảng LCP - Suffix Array).  

**Nhận xét**  
Ý tưởng cơ bản của bài toán là: Ta sẽ xét từng hậu tố một vào tập mình xem xét, kiểm tra xem nếu lấy 1 tiền tố của hậu tố đó, ta sẽ sinh ra được bao nhiêu xâu con phân biệt.  
VD: Với xâu S = "abcca"  
| Vị trí bắt đầu | Xâu hậu tố |  
| --- | --- |  
| 4 | a |  
| 0 | abcca |  
| 1 | bbca |  
| 3 | ca |  
| 2 | cca |  
Gọi tập các xâu con phân biệt là `STR = {}`
- Ta xét xâu hậu tố "a", cập nhật: `STR = {"a"}` 
- Xét đến hậu tố thứ 2 trong SA, xâu "abcca", ta nhận thấy tiền tố "a" của xâu trên đã có trong tập kết quả. Do đó ta không thêm xâu này vào, ngoài ra các tiền tố còn lại đều thêm vào được. Cập nhật: `STR = {"a", "ab", "abc", "abcc", abcca"}`    
- Xét hậu tố tiếp theo là "bbca". Cập nhật `STR =  {"a", "ab", "abc", "abcc", abcca", "b", "bb", "bbc", "bbca"}` 
- Tương tự với hậu tố "ca". Cập nhật `STR =  {"a", "ab", "abc", "abcc", abcca", "b", "bb", "bbc", "bbca", "c", "ca"}`  
- Cuối cùng, tương tự với xâu "abcca", ta thấy tiền tố "c" của xâu "cca" cũng đã xuất hiện trong kết quả, do đó ta không thêm vào. Cập nhật: `STR =  {"a", "ab", "abc", "abcc", abcca", "b", "bb", "bbc", "bbca", "c", "ca", "cc", "cca"}`  

Vậy ta đã liệt kê được hết các xâu con liên tiếp phân biệt của xâu S.  
### **Lời giải**  
Nhận thấy rằng trong ví dụ trên, với `sa[i]`: ta sẽ cập nhật kết quả là `n - sa[i] - lcp[i-1]`.  
Hay nói các khác, nếu ta thêm 1 xâu hậu tố bắt đầu tại vị trí `sa[i]` độ dài `n - sa[i]`, thì ta sẽ tạo thêm được `n - sa[i] - lcp[i-1]` substring mới. Việc này hoàn toàn có thể chứng mình được thông qua nhận xét 1 của bài toán trước. Bạn cũng có thể vẽ xâu ra và hình dung.    
Do đó, kết quả bài toán sẽ là:  

$$\sum_{i = 0}^{n-1}(n - sa[i]) - \sum_{i=0}^{n-2}lcp[i] = n*(n+1)/2 - \sum_{i=0}^{n-2}lcp[i]$$


# II. Bài toán ví dụ  
## [Bài toán 4: Forbidden Indices - CF873F](https://codeforces.com/contest/873/problem/F)  
> Cho 1 xâu S ($1 \leq |S| \leq 200000$) trong đó có 1 vài kí tụ trong số đó là các kí tự cấm. Tìm 1 xâu a sao cho $|a|f(a)$ là lớn nhất. f(a) là số lần xuất hiện của xâu a trong xâu S mà vị trí kết thúc của xâu đó là vị trí không bị cấm.  
VD: S = "aaaa" và vị trí 2 bị cấm, thì xâu "aa" có f("aa") = 2(2 xấu [2..3], [3..4]).  

**Nhận xét**  
Ta có thể lật ngược lại xâu S, bài toán sẽ chuyển về tìm các xấu a sao cho $|a|f(a)$ là lớn nhất và f(a) là số lần xuất hiện của xâu a trong xâu S mà vị trí bắt đầu của xấu đó là vị trí không bị cấm.  
Ta nhận thấy việc đếm số lần xuất hiện của xâu a trong S hoàn toàn có thể giải quyết bằng mảng LCP, số lần xâu a xuất hiện chính là số lần nó là tiền tố của một hậu tố nào đó.  
### **Lời giải**  
Ta chia bài toán thành 2 TH:  
+ $f(a) = 1$: Ta chỉ cần tìm vị trí xa nhất mà nó không phải vị trí bị cấm.  
+ $f(a) > 1$: Với trường hợp này ta sẽ lưu 2 chỉ số L[i], R[i] cho giá trị LCP[i] trên bảng LCP. Với L[i] là vị trí xa nhất mà LCP[i] = min(LCP[L[i]], LCP[L[i] + 1], ... LCP[i]), tương tự với R[i]. Ta hoàn toàn có thể tính toán 2 mảng này trong O(N) (tương tự như bài [KAGAIN SPOJ](https://vn.spoj.com/problems/KAGAIN/)).  

Từ đó số lần xuất hiện của xâu LCP[i] sẽ là `get(L[i], R[i])`, tức là số các vị trí trong khoảng L[i] và R[i] là vị trí không bị cấm. Lưu ý, L[i] và R[i] là các vị trí xuất hiện trong suffix array, không phải vị trí ban đầu.  
[Source code](https://codeforces.com/contest/873/submission/105570902)  

## [Bài toán 5: Fake News - Helvetic Coding Contest 2017](https://codeforces.com/contest/802/problem/I)  
> Cho xâu s ($1 \leq |S| \leq 200000)$. Tính tổng $\sum_p cnt(s, p)^2$ Với p là 1 xâu bất kì và cnt(s, p) là số lần p xuất hiện trong xâu s như là 1 xâu con liên tiếp.  

**Nhận xét**  
Lại là 1 bài toán liên quan đến xâu con liên tiếp, ta hoàn toàn có thể mường tượng ra rằng chúng ta nên dùng Suffix Array trong trường hợp này.  
Cụ thể, ta hãy xét xâu: S = "abcc".  
| Vị trí bắt đầu | Xâu hậu tố |  
| ----- | ----- |   
| 0 | abcc |  
| 1 | bcc |  
| 4 | c |  
|3 | cc |  

Mỗi lần ta duyệt một substring, ta sẽ đếm số lần đã xuất hiện của substring đó và cộng vào kết quả. Cập nhật toàn bộ kết quả trên ta được 1 giá trị `A`.  
VD: Substring "c" xuất hiện 2 lần, lần đầu tiền tìm thấy xâu này, ta sẽ cộng kết quả lên 1, lần thứ 2 cộng kết quả lên 2.    
Vậy 1 substring có xuất hiện k lần, thì kết quả sẽ cộng lên: $1 + 2 + ... + k = \frac{k*(k+1)}{2} = \frac{k^2 + k}{2}$. Ta thấy kết quả này khá gần với kết quả ta mong muốn. là $k^2$.  
Trên thực tế, tổng số lần xuất hiện của tất cả substring sẽ là $n*(n+1)$. Do đó, kết quả của bài toán sẽ là:  

$$ \sum_{p, cnt(s, p) = k}k^2 = 2\sum_{cnt(s, p) = k}{\frac{k * (k + 1)}{2}} - \sum{\frac{k}{2}} = 2A - \frac{n*(n+1)}{2}$$  
Do đó, bằng cách tính A ta sẽ tính được kết quả của bài toán.  

### **Lời giải**  
Để duyệt tất cả substring, ta sẽ duyệt kết quả theo mảng Suffix Array.  
VD: Ta có mảng hậu tố sau (minh họa)  
```
aaa
abc
abc
abc
```  

Ta sẽ duyệt từ trên xuống dưới, từng phần tử 1 của xâu hậu tố. 
Đến kí tự "c" hàng cuối cùng, ta sẽ cộng kết quả vào 1 giá trị k bằng số lần mà xâu con bắt đầu từ vị trí đầu tiên của hàng đến vị trí hiện tại của nó đã xuất hiện (Trong trường hợp này là xâu "abc" và k = 3).  
Tương tự với kí tự "b" ở hàng thứ 3, thì ta thấy xâu "ab" đã xuất hiện 2 lần (tính cả lần xuất hiện của hàng hiện tại). Ta cộng kết quả vào.  
Bằng phương pháp này, ta có thể tính được giá trị A.  
Bài toán này ta có thể truy đổi về 1 bài toán gồm 2 loại truy vấn:  
- 1 tính tổng các giái trị $a_1, a_2,...a_n$  
- Cho giá trị k, tăng các giá trị  từ $a_1,..a_k$ lên 1, set các giá trị từ $a_{k+1}...a_n$ về 0.  

Với n = độ dài xâu, `k = LCP[i]` với $i \leq n-2$.  
Trong trường hợp này, truy vấn một để cộng kết quả cho cả 1 hàng, thay vì cộng từng phần tử 1.  
Truy vấn 2 là truy vấn cập nhật giá trị.  
Bài toán truy vấn này hoàn toàn có thể giải bằng cây IT + lazy propagation.  
Chi tiết bạn có thể xem code mình: [Source code](https://codeforces.com/contest/802/submission/106020829)  





















 

