# 函式
## 課程重點
### a173: 質數日 (PrimeDay)
### 題目概述
判斷一個介於20000000~30000000的8位數是否為質數，與把它的最大位數刪除之後是否為質數，重覆直到變成個位數，如果全部都是，那麼那一天就是質數日。
### 原始程式(理論上不正確)
```cpp=
#include <bits/stdc++.h>
using namespace std;
bool isPrime(int n){
    if(n < 2) return false;
    for(int i = 2; i * i <= n; i++){
        if(n % i == 0){
            return false;
        }
    }
    return true;
}
int main()
{
    int k;
    cin >> k;

    for(int f = 0; f < k; f++){
        int a;
        cin >> a;

        if(a >= 10000000 && a <= 99999999){
            if(isPrime(a)==true){
                cout << a << " is a Prime Day!\n";
            }else{
                cout << a << " isn't a Prime Day!\n";
            }
        }
    }    
    return 0;
}
```
### 正確程式
```cpp=
#include <bits/stdc++.h>
using namespace std;

bool isPrime(int n) {
  for (int i = 2; i < sqrt(n); i++) {
    if (n % i == 0) {
      return false;
    }
  }
  return true;
}

int main() {
  int n;
  cin >> n;

  int date;
  for (int i = 0; i < n; i++) {
    int digit = 10000000;
    cin >> date;
    int t = date, f = 1;
    while (t > 0) {
      if (!isPrime(t)) {
        f = 0;
        cout << date << " isn't a Prime Day!" << endl;
        break;
      }
      t %= digit;
      digit /= 10;
    }
    if (f == 1) {
      cout << date << " is a Prime Day!" << endl;
    }
  }

  return 0;
}

```
### 重點
這題得解題重點在於寫出一個判斷質數的函式，然後再用這個函式來判斷輸入的值，原本應該還需要寫一個重複判斷該八位數字刪掉最大位數之後是否依然是整數，但我發現我雖然少了這個步驟，但是系統還是給我判定為正確，於是我就問AI說在是否所有8位數質數都符合這個要求，但後來發現好像不是，只是單純系統判斷的漏洞而已。
# 遞迴
## 課程重點
### a174: 合成函數(2)
### 題目概述
令f(x)=2x–3；g(x,y)=2x+y–7；h(x,y,z)=3x–2y+z
計算合成函數的值
### 程式
```cpp=
#include <bits/stdc++.h>
using namespace std;
int eval() {
    int x, y, z;
    string part;
    cin >> part;
    if (part == "f") {
        x = eval();
        return 2 * x - 3;
    }
    else if (part == "g") {
        x = eval();
        y = eval();
        return 2 * x + y - 7;
    }
    else if (part == "h") {
        x = eval();
        y = eval();
        z = eval();
        return 3 * x - 2 * y + z;
    }
    else {
        return stoi(part);
    }
}
int main() {
    int ans = eval();
    cout << ans;
    return 0;
}
```
### 重點
這題的解題重點是要判斷輸入的函式代號來決定這個函式需要讀幾個值，然後來計算整個合成函式計算出來的值。
### a175: 二維黑白影像編碼
### 程式
```cpp=
#include <bits/stdc++.h>
using namespace std;
string str;
int num;
int idx = -1;
// 遞迴解析矩陣
// n = 此層代表的矩陣邊長
int matrix(int n) {
    idx++;  // 讀下一個字元

    if (str[idx] == '0') {
        // '0' 表示此區塊全為 0
        return 0;
    } 
    else if (str[idx] == '1') {
        // '1' 表示此區塊全為 1
        // 回傳此區塊的黑色像素數量 = n*n
        return n * n;
    } 
    else if (str[idx] == '2') {
        // '2' 表示再細分四個子區塊
        int cnt = 0;
        for (int i = 0; i < 4; i++) {
            // 每個子區塊尺寸為 n/2
            cnt += matrix(n / 2);
        }
        return cnt;
    }// 理論上不會走到這裡
    return 0;
}
int main() {
    cin >> str;   
    cin >> num;   
    int output = matrix(num);  // 計算總黑色像素數
    cout << output;
    return 0;
}

```
### 重點
這題的解題重點在於用遞迴去慢慢往內縮找出每一個1跟0
