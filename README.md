# R Language

## 基本介紹

---

### 1. 物件類型

- **變數層(Variable)：**
    - 邏輯向量(logical)
    - 整數向量(integer)
    - 因子向量(factor)
    - 數字向量(numeric)
    - 文字向量(character)
- **列表層(List)：**
    - 列表(list)
        - 列表裡面可以同時包含數個陣列層物件及變數層物件
    - S3物件(S3 class)
    - S4物件(S4 class)

- **陣列層(Array)：**
    - 矩陣(matrix)
        
        ```r
        x = 1:4
        X = matrix(x, nrow = 2, ncol = 2)
        ```
        
        - 裡面的每個元素都為單一種類
        - 可進行運算
    - 資料表(data.frame)
        
        ```r
        x = 1:4
        M1 = matrix(x, nrow = 2, ncol = 2)
        b = c(0.7, -0.9, 1.2, -2.1)
        D1 = data.frame(M1)
        ```
        
        - 允許每欄有不同的屬性
        - 一個資料表內可以同時擁有不同屬性的變項
        
        ```r
        a = c(TRUE, FALSE, TRUE, FALSE, FALSE)
        b = c(0.7, -0.9, 1.2, -2.1, 3.7)
        DATA = data.frame(a, b)
        ```
        

### 2. 基本索引 [ ]

- 「中括號」是索引函數 e.g: **`y = c(6, 7, 9, 8, 10)`**
    - 索引指定位置的內容
        - **`y[c(3, 5)]` #**
    - 負數索引：排除該位置內容
        - **`y[-1]`　＃**
    - 指定位置內容運算＆賦值
        - **`y[3] = y[1] + y[2]`**

### 3. 基本函數

- **物件操作:**
    - **class() :** check object’s type
    - **as.欲變換類型():** change object’s type
    - **c() :** 以逗點為界，將不同「數字」合併在同一個物件
        - **`x = c(1, 2, 3, 4, 5)` =** **`x = 1:5`** (因為是連續值)
        - 物件可相互進行運算
    - ! : 所有TRUE與FALSE互相調換
        - **`x = ! dat$Stage %in% 1:5` →** X為TRUE代表為「不屬於1至5的」
- **數學運算:**
    - exp()：自然指數運算
    - sqrt()：平方根運算
    - log() : ****對數運算
    - **`log(4, base = 2) 以4為底 log2` =** **`log(4, 2)`**
- **其他:**
    - help() : 查詢函數功能
        - **`help(read.csv)` =** **`?read.csv`**
    - class(): 確認物件格式
    - cat(”提示文字”) : 能與使用者溝通，與判斷句配合可呈現，當資格符合時，跳出該文字提示的設計
- **變數層(Variable)物件操作：**
    - length(): 查詢向量的長度
    - levels(): 查詢因子向量的種類 →**僅能用在因子向量上**
- **陣列層(Array)物件操作：**
    - length() : 陣列的總長度(長*寬)
    - nrow() / ncol() : 列欄數目
    - 列欄命名 : rownames(x)=c(row1,row2,…)  **/** colnames(x)=c(col1,col2,…)
        - 給定名稱後，能透過索引函數，索引名稱來叫出該物件 →  **`X[,c("E", "C")]`**
    - **矩陣(matrix)**
        - t() : 轉置矩陣
        - solve() : 反矩陣
        - x %*% y : 矩陣乘法
            
            ✅ Notice: `X%*%Y` & `X*Y` 差別: 前者為兩矩陣相乘 ; 後者為 x 矩陣內所有元素*y 運算
            
        - 矩陣索引 x[ row ,  col ]
            - 呼叫全部 (留白)
                
                **`X[3,]`**→代表叫出第3列的全部欄位資料
                
                **`X[,c(2, 5)]`**→ 叫出第2和第5欄位的全部列位資料
                
    - **資料表(data.frame)**
        - $ 函數
            - 索引功能 : X $ 欄位名
            - 新增變數 : X $ 新增欄位名 = c (因子1 , 因子2,…)
        - rbind()  /  cbind() : 擴充陣列層的物件 → **耗時 可用到函數「do.call」，用法是把list裡面的物件當作參數**
            - **`rbind(X, Y)` : 在X列位上併入Y**
            - **`cbind(Y, X)` : 在Y欄位上併入X**

### 4. 特殊函數

- 自訂函數:
    
    name**=function(**x,y**) {**  …. **}**
    
- 進度條:
    
    setTxtProgressBar()  /  txtProgressBar()
    
    ```r
    n = 100
    pb = txtProgressBar(max = n, style=3)
    for(i in 1:n) {
      setTxtProgressBar(pb, i)
    }
    close(pb)
    ```
    

- diff( list欄位) : 計算向量的連續元素之間的差值

## 資料分析

---

### 1. 資料讀入及寫出

- read.csv() : 讀取csv檔

```r
dat = read.csv("filename.csv", header = TRUE, fileEncoding = 'CP950',row.names = FALSE, quote = FALSE)
#【header = TRUE】: 讀取檔案時，該檔案的首列為**欄位名稱
#**【row.names = FALSE】:讀取檔案時不要把列名稱寫出
#【quote = FALSE】: 寫出文字時把雙引號去掉
```

- head(): 查看資料表的前6列
- summary() : 查看此資料表大致結構
- write.csv(): 輸出檔案

```r
write.csv(輸出資料表名稱, "檔名.csv", fileEncoding = 'CP950')
```

### 2. 簡單的資料清理

- X %in% Y : 確認左邊的物件是否有在右邊的物件中出現過 →回傳 True / False
- duplicated() : →回傳 True / False
    - 找尋整個資料表是否有重複的列
        - **`dat$新欄位名稱 = duplicated(dat)`**
    - 檢查單一變數是否重複
        - **`dat$新欄位名稱 = duplicated(dat$school)`**
- all.equal (dat1$x, dat2$y) : 檢查欄位內容是否相同

 

### 3. 驗證規則

數據類型

- 不正確的數據(Incorrect data)
- 不準確的數據(Inaccurate data) →wrong data
- 重複的數據(Duplicate data)
- 不完整的數據(Incomplete data) →有遺漏
    - **`is.na(欄位)` →找出空值者設為true**
    - **`mean(欄位)` → 取平均**
- 不一致的數據(Inconsistent data) →不符合該資料格式
- 違反規則(Rule violations) →錯誤收集(不該收錄)的資料

### 4. 合併資料

- merge() : 合併資料表

```r
merge.dat = merge(data1_clean, data2_clean, by = "想要依據的索引變數欄位名稱", all = TRUE)
```

## 列表層物件

---

- **特性**
    - 列表僅僅是索引，因此為其**增加物件並不需要額外複製的動作**
    - 如果一定要在資料表內完成任務，那**預先創造好一個空的資料表**，讓程式為其填數字較有效率
- length() : 物件長度
- names() : 命名物件
- ls() : 看物件中有哪些東西
- 索引 :
    - `L1**[[**"B"]]` =**`L1$B`**        //B為該資料表名稱
    - **`L1[["B"]][3,1]`  =** **`L1$B[1,2]`   →** L1裡面的B裡面的元素，我們可以繼續使用索引函數
- 特定的S3物件(S3 class) 後，就可以透過自訂函數「print.XXX()」呈現想要的結果 → like 客製化 list 物件
    
    ```r
    Test_list = list(student = c('小明', '小華', '小愛'),score = c(80, 90, 75))
    
    #指定一個物件名稱
    class(Test_list) = 'My_list'   
    
    #編寫特定的「print」函數
    print.My_list =function(Test_list) {
    for (i in 1:length(Test_list[[1]])) {
        cat(Test_list[[1]][i], "的分數為", Test_list[[2]][i], "\n")
      }
    }
    Test_list
    
    ## 小明 的分數為 80 
    ## 小華 的分數為 90 
    ## 小愛 的分數為 75
    ```
    

## 套件應用

---

基本安裝方法 :

```r
install.packages("套件名稱")
library(套件名稱)
```

### 1. data.table

- fread : 讀取資料
    
    **`dat = fread('data3_4.csv', header = TRUE ,`**  **`data.table = FALSE)`**  因為讀取進來的物件格式是「data.table」，故透過**`data.table = FALSE`**解決此項問題
    
    ☀️　另外還有一種儲存、載入物件的方式：R內建的**「save」**與**「load」**函數，可以完全保留物件原有的所有屬性，並且能夠把任何物件存出，也具有較小的儲存空間
    
    ```r
    save(資料表名稱, file = '存出檔案名稱')
    load('載入檔案名稱')
    ```
    

### 2. readJPEG

- 讀取JPG圖片

### 3.magritt

- %>% :從左到右依序執行任務，後面函數的「.」代表上一步的結果
    
    ```r
    #寫法一 : length(levels(factor(dat1$TESTNAME)))
    #寫法二:
    n.TESTNAME = dat1$TESTNAME %>% factor %>% levels %>% length
    #寫法三:
    n.TESTNAME = dat1$TESTNAME %>% factor(.) %>% levels(.) %>% length(.)
    ```
    
    ☀️「.」的應用 :
    
    ```r
    f = function(x, a, b) {a*x^2 + b}
    1:5 %>% f(., 2, 5)
    
    #代表x代1~5，a=2, b=5 進行該函數運算
    ```
    

### 4. dplyr : 資料處理

- mutate : 新增變數

```r
final.data_1 %<>% mutate(輸出欄位名 , 目標欄位名1 * 目標欄位名2 ) #(新變數 , 新變數項進行的欄位運算 ) 
```

☀️ **`%<>%`  :** 改變物件內容

[Final](https://www.notion.so/Final-4b0051ff39ec4e2f9e5af67def7cad19?pvs=21)
