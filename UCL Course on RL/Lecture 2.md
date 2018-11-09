# Lecture 2: Markov Decision Process
## Giới thiệu về MDP
- Một MDP thường mô tả một môi trường trong bài toán Reinforcement Learning (RL)
- Khi mà môi trường có thể quan sát đầy đủ (fully observable), tức là observation của agent cũng chính là state của environment
- Gần như tất cả các bài toán RL đều có thể đưa về dạng MDP

## Tính chất Markov
> Tương lai luôn độc lập với quá khứ khi biết được trạng thái hiện tại.
> 
Một trạng thái ![eq](https://latex.codecogs.com/gif.latex?S_t) được coi là có tính chất Markov nếu:

![eq](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bbmatrix%7D%20P_%7B11%7D%20%26%20...%20%26%20P_%7B1n%7D%5C%5C%20...%20%26%20%26%20%5C%5C%20P_%7Bn1%7D%20%26%20...%20%26%20P_%7Bnn%7D%20%5Cend%7Bbmatrix%7D)

tức là trạng thái này *captures* được tất cả các thông tin liên quan từ quá khứ và một khi biết trạng thái này, các trạng thái quá khứ có thể bỏ đi.

## Ma trận chuyển đổi trạng thái
Nếu ai đã biết qua về Markov Model thì có lẽ không xa lạ với khái niệm Ma trận chuyển đổi trạng thái (State Transition Matrix). Nó là một ma trận ghi lại những xác suất chuyển đổi giữa các cặp trạng thái trong một tập trạng thái hữu hạn.

![eq](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bbmatrix%7D%20P_11%20%26...%20%26P_1n%20%5C%5C%20...%20%26%20%26%20%5C%5C%20P_n1%26...%20%26P_nn%20%5Cend%7Bbmatrix%7D)

với $P[i][j]$ là xác suất chuyển đổi từ trạng thái $i$ sang trạng thái $j$.
## Markov process
Một quy trình Markov (Markov Process) là một quy trình ngẫu nhiên không có bộ nhớ (memoryless random process), tức là một chuỗi các trạng thái có tính Markov ![gif.latex?S_1%2C%20S_2%2C%20...%2C%20S_n](https://latex.codecogs.com/gif.latex?S_1%2C%20S_2%2C%20...%2C%20S_n)

***Definition***
Một quy trình Markov (hay chuỗi Markov) là một bộ (S, P):
- S là một tập hữu hạn các trạng thái có tính Markov
- P là ma trận chuyển đổi trạng thái

Dưới đây là một ví dụ về một quy trình Markov. Từ biểu diễn dưới đây, các chuỗi Markov, hay có thể gọi là **episode** (một chuỗi trạng thái từ lúc bắt đầu đến khi kết thúc), với trạng thái bắt đầu là C1 có thể là:
- C1 C2 C3 Pass Sleep
- C1 FB FB C1 C2 Sleep
- C1 C2 C3 Pub C2 C3 Pass Sleep
- C1 FB FB C1 C2 C3 Pub C1 FB FB FB C1 C2 C3 Pub C2 Sleep

![studentmarkovchain.png](images/studentmarkovchain.png)
![stm.png](images/stm.png)

Dễ dàng nhận ra trong tập trạng thái này, Sleep là trạng thái kết thúc.

## Markov Reward Process (MRP)

Một quy trình có phần thưởng Markov (Markov Reward Process) là một chuỗi Markov kèm theo giá trị phần thưởng (obviously :D)

***Definition***
Một quy trình phần thưởng Markov là một bộ ![gif.latex?%28S%2C%20P%2C%20R%2C%20%5Cgamma%29](https://latex.codecogs.com/gif.latex?%28S%2C%20P%2C%20R%2C%20%5Cgamma%29):
- S là một tập hữu hạn các trạng thái có tính Markov
- P là ma trận chuyển đổi trạng thái và ![gif.latex?P%5Ea_%7Bss%27%7D%20%3D%20P%5BS_%7Bt&plus;1%7D%20%3D%20s%27%20%7C%20S_t%20%3D%20s%5D](https://latex.codecogs.com/gif.latex?P%5Ea_%7Bss%27%7D%20%3D%20P%5BS_%7Bt&plus;1%7D%20%3D%20s%27%20%7C%20S_t%20%3D%20s%5D)
- R là hàm giá trị phần thưởng (reward function), tức  ![gif.latex?R_t%20%3D%20%5Cmathop%7B%7B%7D%5Cmathbb%7BE%7D%7D%5B%7BR_%7Bt&plus;q%7D%20%7C%20S%20%3D%20s_t%7D%5D](https://latex.codecogs.com/gif.latex?R_t%20%3D%20%5Cmathop%7B%7B%7D%5Cmathbb%7BE%7D%7D%5B%7BR_%7Bt&plus;q%7D%20%7C%20S%20%3D%20s_t%7D%5D)
- ![gif.latex?%5Cgamma](https://latex.codecogs.com/gif.latex?%5Cgamma) là giá trị chiết khấu (discounted factor) thuộc khoảng [0, 1]

Quay trở lại với ví dụ trước:
![studentmarkovchainreward.png](images/studentmarkovchainreward.png)

Có thể thấy là tên này rất thích đi Pub và ngủ mà vẫn được qua môn :D 
## Return
***Definition***

Giá trị hồi đáp (return) ![gif.latex?G_t](https://latex.codecogs.com/gif.latex?G_t) là tổng giá trị phần thưởng có chiết khấu tính từ thời điểm t,

![gif.latex?G_t%20%3D%20R_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20*%20R_%7Bt&plus;2%7D%20&plus;%20...%20%3D%20%7B%5Csum%20%5E%5Cinf%7D_%7Bk%3D0%7D%20%28%5Cgamma%5Ek%29*R_%7Bt&plus;k&plus;1%7D](https://latex.codecogs.com/gif.latex?G_t%20%3D%20R_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20*%20R_%7Bt&plus;2%7D%20&plus;%20...%20%3D%20%7B%5Csum%20%5E%5Cinf%7D_%7Bk%3D0%7D%20%28%5Cgamma%5Ek%29*R_%7Bt&plus;k&plus;1%7D)

như đã giải thích ở lecture 1, giá trị discounted value được chọn sao cho phù hợp với bài toán, khi giá trị này gần 0 thì hàm return được gọi là "cận thị" (myopic), tức là chỉ nhìn/dự đoán được reward trong tương lại cực gần. Ngược lại, nếu giá trị này gần 1, hàm return được coi là có tầm nhìn xa (far-sighted).

Lí do tồn tại của discounted value là:
- Tránh trường hợp return trả lại là vô cùng (infinite) do bị rơi vào vòng lặp trong chuỗi Markov.
- Những lỗi dự đoán gây ra bởi độ không chắc chắn (uncertainty) trong tương lai xa sẽ được giảm bớt.
- Có thể tùy chỉnh mục đích của bài toán. Ví dụ, trong bài toán tài chính, phần thưởng tức thời (immediate rewards) được đánh giá cao hơn.

## Value function
***Definition***
Hàm giá trị (Value Function) v(s) của một MRP là giá trị kì vọng của return từ trạng thái s,

![gif.latex?V%28s%29%20%3D%20%5Cmathop%7B%7B%7D%5Cmathbb%7BE%7D%7D%5BG_t%20%7C%20S%20%3D%20s%5D](https://latex.codecogs.com/gif.latex?V%28s%29%20%3D%20%5Cmathop%7B%7B%7D%5Cmathbb%7BE%7D%7D%5BG_t%20%7C%20S%20%3D%20s%5D)

Quay trở lại ví dụ của chúng ta, ta có thể tính được value function cho trạng thái C1 trong các episode được lấy:

![valueexample.png](images/valueexample.png)

Biểu diễn value function trên đồ thị với các giá trị khác nhau của discounted value:

![value1.png](images/value1.png)
![value2.png](images/value2.png)
![value3.png](images/value3.png)

Câu hỏi đặt ra là làm sao từ đồ thị ban đầu và chưa có episode nào ta có thể tính được các giá trị đó? Câu trả lời nằm ở Bellman Equation.

## Bellman equantion trong MRPs

Một giá trị value của value function có thể được phân giải thành 2 phần:
- phần thưởng tức thời $R_{t+1}$
- value function của state tiếp theo có chiết khấu $\gamma * V(S_{t+1})$

![gif.latex?v%28s%29%20%5C%5C%20%3D%20E%5BG_t%20%7C%20S_t%20%3D%20s%5D%20%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20R_%7Bt&plus;2%7D%20&plus;%20%5Cgamma%5E2R_%7Bt&plus;3%7D%20&plus;%20...%20%7C%20S_t%20%3D%20s%5D%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20%28R_%7Bt&plus;2%7D%20&plus;%20%5Cgamma%20R_%7Bt&plus;3%7D%20&plus;%20...%29%20%7C%20S_t%20%3D%20s%5D%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20G_%7Bt&plus;1%7D%20%7C%20S_t%20%3D%20s%5D%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20v%28S_%7Bt&plus;1%7D%29%20%7C%20S_t%20%3D%20s%5D](https://latex.codecogs.com/gif.latex?v%28s%29%20%5C%5C%20%3D%20E%5BG_t%20%7C%20S_t%20%3D%20s%5D%20%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20R_%7Bt&plus;2%7D%20&plus;%20%5Cgamma%5E2R_%7Bt&plus;3%7D%20&plus;%20...%20%7C%20S_t%20%3D%20s%5D%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20%28R_%7Bt&plus;2%7D%20&plus;%20%5Cgamma%20R_%7Bt&plus;3%7D%20&plus;%20...%29%20%7C%20S_t%20%3D%20s%5D%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20G_%7Bt&plus;1%7D%20%7C%20S_t%20%3D%20s%5D%5C%5C%20%3D%20E%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20v%28S_%7Bt&plus;1%7D%29%20%7C%20S_t%20%3D%20s%5D)

![bellman1.png](images/bellman1.png)

Bellman Equation có thể được diễn giải bằng phép nhân ma trận: ![gif.latex?v%20%3D%20R%20&plus;%20%5Cgamma%20P%20v](https://latex.codecogs.com/gif.latex?v%20%3D%20R%20&plus;%20%5Cgamma%20P%20v) hay chính là:

![gif.latex?%5Cbegin%7Bbmatrix%7D%20v%281%29%5C%5C%20...%5C%5C%20v%28n%29%20%5Cend%7Bbmatrix%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20R_1%5C%5C%20...%5C%5C%20R_n%20%5Cend%7Bbmatrix%7D%20&plus;%20%5Cgamma%20%5Cbegin%7Bbmatrix%7D%20P_%7B11%7D%20%26%20...%20%26%20P_%7B1n%7D%5C%5C%20...%20%26%20%26%20%5C%5C%20P_%7Bn1%7D%20%26%20...%20%26%20P_%7Bnn%7D%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20v%281%29%5C%5C%20...%5C%5C%20v%28n%29%20%5Cend%7Bbmatrix%7D](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bbmatrix%7D%20v%281%29%5C%5C%20...%5C%5C%20v%28n%29%20%5Cend%7Bbmatrix%7D%20%3D%20%5Cbegin%7Bbmatrix%7D%20R_1%5C%5C%20...%5C%5C%20R_n%20%5Cend%7Bbmatrix%7D%20&plus;%20%5Cgamma%20%5Cbegin%7Bbmatrix%7D%20P_%7B11%7D%20%26%20...%20%26%20P_%7B1n%7D%5C%5C%20...%20%26%20%26%20%5C%5C%20P_%7Bn1%7D%20%26%20...%20%26%20P_%7Bnn%7D%20%5Cend%7Bbmatrix%7D%20%5Cbegin%7Bbmatrix%7D%20v%281%29%5C%5C%20...%5C%5C%20v%28n%29%20%5Cend%7Bbmatrix%7D)

Có thể thấy Bellman Equation là một phương tình tuyến tính và có thể giải trực tiếp bằng cách biến đổi:

![gif.latex?v%20%3D%20R%20&plus;%20%5Cgamma%20P%20v%5C%5C%20%281%20-%20%5CgammaP%29v%20%3D%20R%5C%5C%20v%20%3D%20%281-%5Cgamma%20P%29%5E%7B-1%7D%20R](https://latex.codecogs.com/gif.latex?v%20%3D%20R%20&plus;%20%5Cgamma%20P%20v%5C%5C%20%281%20-%20%5CgammaP%29v%20%3D%20R%5C%5C%20v%20%3D%20%281-%5Cgamma%20P%29%5E%7B-1%7D%20R)

Độ phức tạp tính toán là ![gif.latex?O%28n%5E3%29](https://latex.codecogs.com/gif.latex?O%28n%5E3%29) với n trạng thái, nên chỉ dùng với những bài toán MRP nhỏ. Với những bài toán ứng bới MRP lớn hơn, có nhiều phương pháp khác để tìm (hay xấp xỉ) của hàm giá trị như:
- Dynamic Programming
- Monte-Carlo evaluation
- Temporal-Difference learning

## Markov Decision Process
Có thể bạn sẽ thắc mắc tại sao suốt từ đầu lecture đến giờ không có sự xuất hiện của tập các actions, thì câu trả lời là nó nằm ở Markov Decision Process (MDP). Một quy trình Markov quyết định là một quy trình Markov có phần thưởng và kèm theo các hành động (hay các quyết định). Nó là một môi trường mà tất cả trạng thái đều có tính Markov.

***Definition***
Một MDP được biểu diễn bằng một tuple ![gif.latex?%3CS%2C%20A%2C%20P%2C%20R%2C%20%5Cgamma%3E](https://latex.codecogs.com/gif.latex?%3CS%2C%20A%2C%20P%2C%20R%2C%20%5Cgamma%3E), trong đó:
- S là một tập hữu hạn các trạng thái có tính Markov
- P là ma trận chuyển đổi trạng thái và ![gif.latex?P%5Ea_%7Bss%27%7D%20%3D%20P%5BS_%7Bt&plus;1%7D%20%3D%20s%27%20%7C%20S_t%20%3D%20s%2C%20A_t%20%3D%20a%5D](https://latex.codecogs.com/gif.latex?P%5Ea_%7Bss%27%7D%20%3D%20P%5BS_%7Bt&plus;1%7D%20%3D%20s%27%20%7C%20S_t%20%3D%20s%2C%20A_t%20%3D%20a%5D)
- A là tập hữu hạn các hành động
- R là hàm giá trị phần thưởng (reward function), tức  ![gif.latex?R_t%20%3D%20%5Cmathop%7B%7B%7D%5Cmathbb%7BE%7D%7D%5B%7BR_%7Bt&plus;q%7D%20%7C%20S%20%3D%20s_t%7D%5D](https://latex.codecogs.com/gif.latex?R_t%20%3D%20%5Cmathop%7B%7B%7D%5Cmathbb%7BE%7D%7D%5B%7BR_%7Bt&plus;q%7D%20%7C%20S%20%3D%20s_t%7D%5D)
- ![gif.latex?%5Cgamma](https://latex.codecogs.com/gif.latex?%5Cgamma) là giá trị chiết khấu (discounted factor) thuộc khoảng [0, 1]

![mdp.png](images/mdp.png)

## Policies 

Policy ![gif.latex?%5Cpi](https://latex.codecogs.com/gif.latex?%5Cpi) của một MDP là một phân phối có điều kiện trên tập actions và states:

![gif.latex?%5Clarge%20%5Cpi%28a%7Cs%29%20%3D%20P%5BA_t%20%3D%20a%20%7C%20S_t%20%3D%20a%5D](https://latex.codecogs.com/gif.latex?%5Clarge%20%5Cpi%28a%7Cs%29%20%3D%20P%5BA_t%20%3D%20a%20%7C%20S_t%20%3D%20a%5D)

- Cho một MDP ![gif.latex?M%20%3D%20%3CS%2C%20A%2C%20P%2C%20R%2C%20%5Cgamma%3E](https://latex.codecogs.com/gif.latex?M%20%3D%20%3CS%2C%20A%2C%20P%2C%20R%2C%20%5Cgamma%3E) và một policy ![gif.latex?%5Cpi](https://latex.codecogs.com/gif.latex?%5Cpi)
- Chuỗi các state vầ reward ![gif.latex?s_1%2C%20r_1%2C%20s_2%2C%20r_2%2C%20...](https://latex.codecogs.com/gif.latex?s_1%2C%20r_1%2C%20s_2%2C%20r_2%2C%20...) là một MRP ![gif.latex?%3CS%2C%20P%5E%5Cpi%2C%20R%5E%5Cpi%2C%20%5Cgamma%3E](https://latex.codecogs.com/gif.latex?%3CS%2C%20P%5E%5Cpi%2C%20R%5E%5Cpi%2C%20%5Cgamma%3E) với
 
![gif.latex?%5Clarge%20P%5E%5Cpi%28s%2C%20s%27%29%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29%20P%5Ea_%7Bss%27%7D](https://latex.codecogs.com/gif.latex?%5Clarge%20P%5E%5Cpi%28s%2C%20s%27%29%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29%20P%5Ea_%7Bss%27%7D)

![gif.latex?R%5E%5Cpi_s%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29%20R%5Ea_s](https://latex.codecogs.com/gif.latex?R%5E%5Cpi_s%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29%20R%5Ea_s)

## Value function 

Hàm state-value ![gif.latex?v_%5Cpi%28s%29](https://latex.codecogs.com/gif.latex?v_%5Cpi%28s%29) của một MDP là giá trị expected returnn từ trạng thái s và đi theo policy ![gif.latex?%5Cpi](https://latex.codecogs.com/gif.latex?%5Cpi):

![gif.latex?v_%5Cpi%28s%29%20%3D%20E_%5Cpi%20%5BG_t%20%7C%20S_t%20%3D%20s%5D](https://latex.codecogs.com/gif.latex?v_%5Cpi%28s%29%20%3D%20E_%5Cpi%20%5BG_t%20%7C%20S_t%20%3D%20s%5D)

Tương tự, ta cũng định nghĩa ra hàm action-value ![gif.latex?q_%5Cpi%28s%2C%20a%29](https://latex.codecogs.com/gif.latex?q_%5Cpi%28s%2C%20a%29), là giá trị expected returnn từ trạng thái s, thực hiện action a và sau đó đi theo policy ![gif.latex?%5Cpi](https://latex.codecogs.com/gif.latex?%5Cpi):

![gif.latex?q_%5Cpi%28s%2C%20a%29%20%3D%20E_%5Cpi%20%5BG_t%20%7C%20S_t%20%3D%20s%2C%20A_t%20%3D%20a%5D](https://latex.codecogs.com/gif.latex?q_%5Cpi%28s%2C%20a%29%20%3D%20E_%5Cpi%20%5BG_t%20%7C%20S_t%20%3D%20s%2C%20A_t%20%3D%20a%5D)

Và ta cũng có thể áp dụng Bellman Equation với MDP:

![gif.latex?v_%5Cpi%20%28s%29%20%3D%20E_%5Cpi%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20v_%5Cpi%28S_%7Bt&plus;1%7D%29%20%7C%20S_t%20%3D%20s%5D](https://latex.codecogs.com/gif.latex?v_%5Cpi%20%28s%29%20%3D%20E_%5Cpi%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20v_%5Cpi%28S_%7Bt&plus;1%7D%29%20%7C%20S_t%20%3D%20s%5D)

![gif.latex?q_%5Cpi%20%28s%2C%20a%29%20%3D%20E_%5Cpi%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20q_%5Cpi%28S_%7Bt&plus;1%7D%2C%20A_%7Bt&plus;1%7D%29%20%7C%20S_t%20%3D%20s%2C%20A_t%20%3D%20a%5D](https://latex.codecogs.com/gif.latex?q_%5Cpi%20%28s%2C%20a%29%20%3D%20E_%5Cpi%5BR_%7Bt&plus;1%7D%20&plus;%20%5Cgamma%20q_%5Cpi%28S_%7Bt&plus;1%7D%2C%20A_%7Bt&plus;1%7D%29%20%7C%20S_t%20%3D%20s%2C%20A_t%20%3D%20a%5D)

Mối quan hệ giữa hàm state-value và action-value được thể hiện bởi công thức:

![gif.latex?v_%5Cpi%20%28s%29%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29q_%5Cpi%28s%2C%20a%29](https://latex.codecogs.com/gif.latex?v_%5Cpi%20%28s%29%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29q_%5Cpi%28s%2C%20a%29)

mặt khác, do:

![gif.latex?%5Clarge%20q_%5Cpi%28s%2Ca%29%20%3D%20R%5Ea_s%20&plus;%20%5Cgamma%5Csum%20_%7Bs%27%5Cepsilon%20S%7DP%5Ea_%7Bss%27%7D%20v_%5Cpi%28s%27%29](https://latex.codecogs.com/gif.latex?%5Clarge%20q_%5Cpi%28s%2Ca%29%20%3D%20R%5Ea_s%20&plus;%20%5Cgamma%5Csum%20_%7Bs%27%5Cepsilon%20S%7DP%5Ea_%7Bss%27%7D%20v_%5Cpi%28s%27%29)

nên ta có:

![gif.latex?%5Clarge%20v_%5Cpi%20%28s%29%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29%5BR%5Ea_s%20&plus;%20%5Cgamma%5Csum%20_%7Bs%27%5Cepsilon%20S%7DP%5Ea_%7Bss%27%7D%20v_%5Cpi%28s%27%29%5D](https://latex.codecogs.com/gif.latex?%5Clarge%20v_%5Cpi%20%28s%29%20%3D%20%5Csum%20_%7Ba%5Cepsilon%20A%7D%20%5Cpi%28a%7Cs%29%5BR%5Ea_s%20&plus;%20%5Cgamma%5Csum%20_%7Bs%27%5Cepsilon%20S%7DP%5Ea_%7Bss%27%7D%20v_%5Cpi%28s%27%29%5D)

và:

![gif.latex?%5Clarge%20q_%5Cpi%28s%2Ca%29%20%3D%20R%5Ea_s%20&plus;%20%5Cgamma%5Csum%20_%7Bs%27%5Cepsilon%20S%7DP%5Ea_%7Bss%27%7D%20v_%5Cpi%28s%27%29%5Csum%20_%7Ba%27%5Cepsilon%20A%7D%5Cpi%28a%27%7Cs%27%29q_%5Cpi%28s%27%2C%20a%27%29](https://latex.codecogs.com/gif.latex?%5Clarge%20q_%5Cpi%28s%2Ca%29%20%3D%20R%5Ea_s%20&plus;%20%5Cgamma%5Csum%20_%7Bs%27%5Cepsilon%20S%7DP%5Ea_%7Bss%27%7D%20v_%5Cpi%28s%27%29%5Csum%20_%7Ba%27%5Cepsilon%20A%7D%5Cpi%28a%27%7Cs%27%29q_%5Cpi%28s%27%2C%20a%27%29)

Hơi rắc rối một chút, nhưng ta đã có được hai công thức Bellman Equation của state-value và action-value function :D

Biểu diễn dưới dạng ma trận và phương pháp giải Bellman Equation của MDP cũng tương tự như với MRP.

## Optimal Value Function

Hàm state-value tối ưu tại một trạng thái là hàm giá trị có giá trị lớn nhất tại trạng thái đó trên toàn bộ các policies:

![gif.latex?%5Clarge%20v_*%28s%29%20%3D%20%5Cmax_%5Cpi%20v_%5Cpi%20%28s%29](https://latex.codecogs.com/gif.latex?%5Clarge%20v_*%28s%29%20%3D%20%5Cmax_%5Cpi%20v_%5Cpi%20%28s%29)

tương tự với action-value function:

![gif.latex?%5Clarge%20q_*%28s%2C%20a%29%20%3D%20%5Cmax_%5Cpi%20q_%5Cpi%20%28s%2C%20a%29](https://latex.codecogs.com/gif.latex?%5Clarge%20q_*%28s%2C%20a%29%20%3D%20%5Cmax_%5Cpi%20q_%5Cpi%20%28s%2C%20a%29)

Một bài toán MDP coi là "được giải" khi ta tìm được các hàm tối ưu trên.

## Optimal policy

Ta kí hiệu rằng: ![gif.latex?%5Cpi%20%3E%20%5Cpi%27%20%5C%2C%5C%2Cif%5C%2C%5C%2C%20v_%5Cpi%28s%29%20%3E%20v_%5Cpi%27%28s%29%2C%20%5Cforall%20s](https://latex.codecogs.com/gif.latex?%5Cpi%20%3E%20%5Cpi%27%20%5C%2C%5C%2Cif%5C%2C%5C%2C%20v_%5Cpi%28s%29%20%3E%20v_%5Cpi%27%28s%29%2C%20%5Cforall%20s)

Với mọi MDP:
- Có một optimal policy tốt hơn hoặc bằng tất cả các policy khác: 
![gif.latex?%5Cpi_*%20%5Cgeq%20%5Cpi%2C%20%5Cforall%20%5Cpi](https://latex.codecogs.com/gif.latex?%5Cpi_*%20%5Cgeq%20%5Cpi%2C%20%5Cforall%20%5Cpi)
- Tất cả các optimal policy đều đưa đến optimal value function: 
![gif.latex?%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%20v_%5Cpi_*%28s%29%20%3D%20v_*%28s%29%20%26%20%26%20%5C%5C%20q_%5Cpi_*%28s%2C%20a%29%20%3D%20q_*%28s%2C%20a%29%20%26%20%26%20%5Cend%7Bmatrix%7D%5Cright.](https://latex.codecogs.com/gif.latex?%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%20v_%5Cpi_*%28s%29%20%3D%20v_*%28s%29%20%26%20%26%20%5C%5C%20q_%5Cpi_*%28s%2C%20a%29%20%3D%20q_*%28s%2C%20a%29%20%26%20%26%20%5Cend%7Bmatrix%7D%5Cright.)

## Finding the optimal policy 

Có thể tìm optimal policy bằng cách maximize hàm action-value trên các cặp state và action:

![gif.latex?%5Cpi_*%28a%7Cs%29%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%201%20%5C%2C%5C%2C%5C%2C%20if%5C%2C%5C%2C%20a%20%3D%20%5Ctextrm%7Bargmax%7D%5C%2C%5C%2C%20q_*%28s%2C%20a%29%20%5C%5C%200%20%5C%2C%5C%2C%5C%2C%20%5Cmathrm%7Botherwise%7D%20%5Cend%7Bmatrix%7D%5Cright.](https://latex.codecogs.com/gif.latex?%5Cpi_*%28a%7Cs%29%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D%201%20%5C%2C%5C%2C%5C%2C%20if%5C%2C%5C%2C%20a%20%3D%20%5Ctextrm%7Bargmax%7D%5C%2C%5C%2C%20q_*%28s%2C%20a%29%20%5C%5C%200%20%5C%2C%5C%2C%5C%2C%20%5Cmathrm%7Botherwise%7D%20%5Cend%7Bmatrix%7D%5Cright.)

do đó, nếu ta biết được optimal action-value function, ta có thể tìm ra ngay optimal policy.

## Bellman Optimality Equation 

Bellman equation cũng áp dụng cho cả các optimal function, mình sẽ để cho mọi người tự hình dung ra nhé :D

## Extensions to MDPs

- Infinite and continuous MDPs.
- Partially observable MDPs.
- Undiscounted, average rewards MDPs.