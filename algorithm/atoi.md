# 8. String to Integer (atoi)

https://leetcode.com/problems/string-to-integer-atoi/

### 이 문제 풀이의 핵심은 String 처리 입니다.

* -, + , 공백이 각각 어느 위치에 오느냐에 따라 결과가 달라집니다.
* int형의 범위가 어디까지인지 알아야 할 필요성이 있습니다.
* 부호 처리에 대한 센스를 물어봅니다.

```c++
int rangeCheck(int num, int adding) {
        long long int l_num = (long long int)num * 10 + adding;
        if ( l_num < INT_MIN ) return INT_MIN;
        else if ( l_num > INT_MAX ) return INT_MAX;
        else return (int) l_num;
}
    
int myAtoi(string str) {
        cout << INT_MIN << endl;
        bool isEmpty = true;
        bool isInit = true;
        int result = 0;
        int p_n = 1;
        for(auto it=str.begin(); it < str.end(); it++) {
            cout << *it << endl;
            if(result == INT_MAX || result == INT_MIN) return result;
            else if(*it == ' ' && isEmpty) continue;
            else if(*it == '-' && isEmpty) {
                p_n *= -1;
                isEmpty = false;
            } else if(*it == '+' && isEmpty) {
                isEmpty = false;
                continue;
            } else if(*it-48 < 0 || *it-48 > 9) return result;
            else {
                int adding = *it - 48;
                if (p_n < 0) adding *= -1;
                result = rangeCheck(result, adding);
                isInit = false;
                isEmpty = false;
            }
        }
        return result;
}
```

