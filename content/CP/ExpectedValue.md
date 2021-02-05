---
title: "Expected value - Giá trị kì vọng"
date: 2021-02-05T14:16:16+09:00
draft: false
---  
# Giới thiệu  
Giá trị kì vọng luôn là một khái niệm quan trọng trong toán học và lập trình thi đấu. Bài viết này là bài viết dịch có chọn lọc từ blog trên [codechef](https://www.codechef.com/wiki/tutorial-expectation) và mình sẽ đưa thêm 1 vài bài toán ứng dụng cơ bản.  
Bản thân mình cũng không quá thân thuộc với các định nghĩa toán học của giá trị kì vọng, nên mình sẽ không thể dịch hoàn toàn sát nghĩa như các bài dịch chuyên nghiệp khác được, cộng với việc đây là bài viết với mục đích tự học nên hi vọng độc giả có thể thông cảm.  

# I. Lý thuyết  
Với 1 biến rời rạc X (discrete variable) ta có hàm xác xuất P(x) là xác xuất xảy ra X. Ta gọi E(x) là giá trị kì vọng của biến X.  
$$E(X) = \sum x_i P(x_i)$$  
Với $x_i$ là các biến con của $X$.  
VD: Cho việc tung xúc xắc 1 lần, ta được các kết quả khả dĩ(possible outcome) sau: {1,2,3,4,5,6} và xác xuất để được từng kết quả đó là 1.  
Do đó, ta nói giá trị kì vọng của số điểm của 1 lần tung xúc xắc sẽ là:  
$E(X) = \sum x_i P(x_i) =  \sum_{x_i = 1}^{6} x_i * \frac{1}{6} = 3.5$  

Ngoài ra còn có 1 luật khá quan trọng là luật giá trị kì vọng tuyến tính: $E[X1 + X2] = E[X1] + [X2]$ với X1, X2 là 2 biến độc lập.  

# II. Các bài toán  
## 1. Giá trị kì vọng của số lần tung đồng xu để được mặt ngửa.  
Gọi giá trị kì vọng của số lần tung đồng xu là $x$. Khi đó ta có thể viết được công thức của nó.  
Ta xét 2 trường hợp:  
+ Nếu lần đầu tung, ta được ngay mặt ngửa, thì ta đã hoàn thành, xác xuất để việc này xảy ra là 1/2 và số lần để được mặt ngửa là 1.  
+ Ngược lại, nếu lần đầu tung ta được mặt up, thì ta sẽ tiếp tục bài toán: Xác xuất xảy ra vẫy là 1/2 và số lần cần đạt sẽ là **$x + 1$**.  

Giá trị kì vọng là tổng giá trị của cả 2 trường hợp trên.  
Ta có công thức:  
$$x = (1/2)(1) + (1/2) (1+x)$$
Giải phương trình trên, ta được $x = 2$.  

## 2. Giá trị kì vọng để được 2 lần tung liên tiếp là mặt ngửa.  
Gọi giá trị kì vọng của số lần tung đồng xu được 2 mặt ngửa liên tiếp là $x$.  
Tương tự như bài toán 1, ta xét 3 trường hợp:  
+ Nếu lân đầu tung ta được mặt úp, thi ta sẽ tiếp tục bài toán: Xác xuất xảy ra là 1/2 và số lần để hoàn thành là $x + 1$.  
+ Nếu lần tung đầu ta được mặt ngửa, nhưng lần tung thứ 2 lại được mặt úp, thì ta vẫn sẽ phải tiếp tục tung xu: Xác xuất xảy ra 1/4 và số lần hoàn thành là $x+2$.  
+ Nếu tung 2 lần liên tiếp được luôn: Ta sẽ kết thúc bài toán: Xác xuất xảy ra là 1/4 và số lần tung đồng xu là 2.  
Ta được phương trình:  
$$x = (1/2)(x+1) + (1/4)(x+2) + (1/4)2$$  
Giải phương trình, ta tính được x = 6.  

## 3. (Trường hợp tổng quát) Giá trị kì vọng số lần tung đồng xu để được N lần liên tiếp mặt ngửa.  
Ta xét N + 1 trường hợp:  
+ Nếu lần tung lần đầu tiên được mặt xấp: Ta thêm vào được phương trình (1/2) * (x + 1).  
+ Nếu lần tung thứ 2 là lần đầu tiên được mặt xấp: Ta thêm vào vế phải phương trình: (1/4)(x+2).  
+ Nếu lần tung thứ 3 là lần đầu tiên được mặt xấp: Ta thêm vào vế phải phương trình: (1/8)(x+3).  
+ ...  
+ Nếu lần tung thứ N là lần đâu tiên được mặt xấp: Ta thêm vào phương trình: $\frac{1}{2^N} (x + N)$  
+ Nêu tung N lần liên tiếp đề được mặt ngửa: Ta thêm vào phương trình: 
$\frac{1}{2^N} N$  
Từ đó ta được phương trình:  
$x =\sum_{t = 1}^{n} \frac{1}{2^t} (x + t) + \frac{N}{2^N}$  

Giải phương trình, ta sẽ tính được $x = 2^{N+1} - 2$  

## 4. Bài toán sinh sản của ong.  
> Ong chúa đang vào mùa sinh sản để duy trì nòi giống. Xác xuất để sinh ra 1 con ong đực là p. Hỏi giá trị kì vọng của số lần sinh để được 1 con ong đực.  

Bài toán này có hướng đi tương tự như bài toán 1.  
Ta gọi x là giá trị kì vọng của số lần sinh.  
Ta có phương trình $x = p + (1 - p)(x + 1)$. Giải phương trình này ta thấy $x = 1/p$.  
Tổng quát hóa: 
> Nếu có K sự kiện, 1 sự kiện là kết quả mong muốn, và K-1 sự kiện còn lại là không mong muốn và xác xuất để đạt được kết quả mong muốn là p. Thì số lần thử để được kết quả mong muốn sẽ là 1/p.  

## 5. Giá trị kì vọng số lần tung xúc xắc để được mặt 4.  
Áp dụng công thức của bài trước với p = 1/6. Thì giá trị kì vọng sẽ là: 1/p = 6 lần tung.  

## 6. Bài toán nhân công.  
> Có rất nhiều ưng viên tham gia phỏng vấn. Xác xuất để ứng viên thứ k được chọn là 1/(k+1). Hỏi giá trị kì vọng số người phỏng vấn để chọn được ít nhất 1 người.  

Tương tự như bài toán 1, ta xét từng trường hợp.  
+ TH1: Người đầu tiên được chọn, xác xuất là 1/2 và số lần phỏng vấn là 1.  
+ TH2: Người thứ 2 được chọn, xác xuất là 1/6 và số lần phỏng vấn là 2.  
+ TH3: Người thứ 3 được chọn, xác xuất xảy ra là: 1/2 * 2/3 * 1/4 và số lần phỏng vấn là 2.  
+ ...    
+ TH k: Người thứ k được chọn, xác xuất là 1/2 * 2/3 * 3/4 * ... * (k-1)/k * 1/(k+1) = 1/ (k * (k+1)) và số lần phỏng vấn là k.  

Ta được phương trình:  

$$x = 1/(1.2) + 2/(2.3) + k/(k.(k+1)) = 1/2 + 1/3 + 1/4 + ...$$  

Đây là 1 dãy phân kì, tức là tổng của chúng không hội tụ, do đó không tồn tại giá trị kì vọng.  

## 7. Bài toán xắp xếp hoán vị.  
> Cho 1 hoán vị P của dãy [1...n] cần được sắp xết theo thứ tự tăng dần. Ở mỗi bước, bạn sẽ chọn 1 cặp (i,j) bất kì, thỏa mãn: $i < j$ và $P[i] > P[j]$. Hỏi giá trị kì vọng số bước cần thiết để sắp xếp dãy.  

Đây là 1 bài toán lập trình, do xác xuất để đổi 2 cặp là như nhau.  
Do đó, giá trị kì vọng sẽ là:  
$$E[P] = 1/cnt * (E[P_s] + 1)$$  

Với cnt là số cặp (i,j) thỏa mãn đề bài và $P_S$ là kết quả của dãy sau khi thực hiện việc 1 thao tác đổi `s`.  
Do đó độ phức tạp sẽ là O(N!).  

## 8. Tung đồng xu N lần, tính giá trị kì vọng của số lần tung được mặt ngửa.  
Gọi $a_i$ là kết quả của lần tung thứ i, $a_i = 1$ khi tung được mặt ngửa và $a_i = 0$ khi tung được mặt up.  
Dễ thấy $E[A_i] = 1/2$.  
Và bởi vì các sự kiện $a_1, a_2, .. a_n$ là độc lập, nên ta có công thức.  
E[số lần được mặt ngửa] = $E[a_1] + E[a_2] + ... E[a_n] = n/2$  
## 9. Phép thử Bernaulli
> Cho n học sinh được chọn các số từ 1 đến 100. Hỏi giá trị kì vọng số học sinh sẽ chọn các số từ 1 đến 9.  

Bài toán này dựa trên khái niệm **phép thử Bernaulli**. 1 thí nghiệm được gọi là phép thử Bernaulli nếu nó có chính xác 2 kết quả, 1 trong số đó là kết quả mong muốn. Bài toán 4 và 5 đều là các phép thử **Bernaulli**.  
Bài toán này dựa trên kết quả của **phép thử Bernaulli**.  
> Nếu xác xuất thành công là p. Thì giá trị kì vọng số lần thành công sau n lần thử sẽ là n * p.  

Tương tự như bài toán 8, ta hoàn toàn có thể tính được giá trị kì vọng số lần thành công của lần thử thứ i: $E_i = 1 * p + (1 - p) * 0 = p$. Do đó kết quả của n lần thử sẽ là n * p.  

Do đó, với bài toán này, ta sẽ được kết quả là n*9/100.  

## 10. Giá trị kì vọng số lần tung đồng xu đề có ít nhất N đồng ngửa  
Ta có thể giải bài toán bằng khái niệm truy hồi.  
Gọi E(N) là số lần tung đồng xu để được N mặt ngửa.  
Ta xét 2 trường hợp:  
+ Nếu lần tung đầu tiên được mặt ngửa. Ta còn N - 1 lần tung mặt ngửa nữa.  
+ Nếu lần tung đầu tiên được mặt úp, ta vẫn còn nguyên N lần tung.  
Từ đó:  
$$E[n] = (1/2)(E[n-1] + 1) + (1/2)(E[n] + 1)$$
Hay: $E[n] = E[n-1] + 2$.  
Trường hợp cơ bản: E[1] = 2(Bài toán 2).  
Từ đó, dễn thấy E[n] = 2*n.  

## 11. Phép thử Bernaulli (Trường hợp tổng quát của bài toán 10) 
> Cho xác xuất thành công của mỗi lần thử là p. Tính giá trị kì vọng số lần thử để được ít nhất n lần thành công.  

Đây là trường hợp tổng quát của bài 10, ta giải bằng phương pháp tương tự.  
Ta có công thức đệ quy.  
$$E[N] = p(E[n-1] + 1) + (1 - p)(E[n] + 1)$$  
Giải phương trình ta được: $E[N] = E[N-1] + 1/p$.  
Mà E[1] = 1/p. Ta tính được E[N] = n/p.  
Phát biểu cho phép thử Bernaulli:    
> Nếu xác xuất thành công là p, thì số lần thử ít nhất để được n lần đúng là n/p.  

# 3. Các bài tập áp dụng  
Dưới đây là danh sách 1 số bài tập thú vị:  
- [Sugoroku 2 - Atcoder](https://atcoder.jp/contests/abc189/tasks/abc189_f)  
- [Checkpoint - CF](https://codeforces.com/contest/1453/problem/D)  
- [Steps to One - CF](https://codeforces.com/problemset/problem/1139/D)  
- [Surrounded Nodes - Atcoder](https://atcoder.jp/contests/abc149/tasks/abc149_f)  
- [Fork in the Road - Atcocder](https://atcoder.jp/contests/abc144/tasks/abc144_f)  
- [Moving Robots - CSES](https://cses.fi/problemset/task/1726/)  


























