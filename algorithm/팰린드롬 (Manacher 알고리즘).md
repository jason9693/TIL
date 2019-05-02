# 팰린드롬 (Manacher 알고리즘) 
 [https://leetcode.com/problems/longest-palindromic-substring](https://leetcode.com/problems/longest-palindromic-substring) 

* Big-O : O(n)
* 배열 A : 
	* A[i] = i번째 문자를 중심으로 하는 가장 긴 회문의 반지름 크기.
		* ex ) ababac -> A[1] = 2
* 배열 S : 문자열

* 동작 순서
	1. i의 탐색 범위는 [1, len(S)]이다.
	2. 모든 j < i에 대해 r = max(j + A[j]) = p + A[p]
	3. i 와 r 의 대소 관계에 따라 A[i]의 초깃값이 결정된다.
		1. i > r : A[i] = 0
		2. i <= r : 
			* i_prime = 2p - i = p + (p-i) = **p - {p -> i 까지의 길이}**
			* A[i] = min(r-i, A[i_prime]) : (p의 반지름의 길이 - (p -> i 까지의 길이)) vs  ( i의 대칭점의 최대 반지름 길이. )
			* i는 p를 중심으로 한 회문에 속한다.
	4. A[i]의 초깃값이 결정되고, S[i - A[i]]와 S[i+A[i]]가 같을때까지, A[i]를 증가시킨다.

* 짝수 팰린드롬
-> 사이사이에 문자열 토큰 삽입.

```
string addTocken(string s) {
        string result = "";
        for(auto it = s.begin() ; it < s.end(); it++) {
            result.append("#");
            result.push_back(*it);
        }
        return result;
    }
    
    int max(int left, int right) {
        if( right > left ) return right;
        return left;
    }
    
    int min(int left, int right) {
        if ( right > left ) return left;
        return right;
    }
    
    string search(string s) {
        string result = "";
        
        int A[s.size()] = {0};
        int p = 0;
        for(int i = 1; i < s.size(); i++) {
            int j = i-1;
            if ( j + A[j] > p + A[p] ) p = j;
            int r = p + A[p];
            
            if( i > r ) A[i] = 0;
            else {
                int i_prime = 2*p - i;
                A[i] = min(r-i, A[i_prime]);
            }
            
            //out of range검사 필수, 본인 인덱스(i-0 == i+0)는 계산하지 않는다.
            while(i - A[i] - 1 >= 0 &&
                  i + A[i] + 1 < s.size() &&    
                  s[i-A[i]-1] == s[i+A[i]+1]) {
                A[i]++;
            }
            
        }
        
        p = 0;
        for (int i = 0; i< s.size(); i++) 
            if (A[i] >= A[p]) p = i;
        
        
        for (int i = p - A[p]; i<= p + A[p]; i++) {
            if(s[i] != '#') result.push_back(s[i]);
        }
        
        return result;
    }
    
    string longestPalindrome(string s) {
        if (s.size() == 0) return s;
        string odd_s = addTocken(s); //짝수케이스, 홀수케이스 모두 계산한 뒤 가장 긴 결과를 고른다.
        
        string result = search(s);
        string result_odd = search(odd_s);
                
        if(result.size() > result_odd.size()) return result;
        return result_odd;
    }
```