# 임시 - sw expert 문제

### 2070 - 큰 놈, 작은 놈, 같은 놈 D1

**내 코드**

```c++
#include<iostream>

using namespace std;

int main(int argc, char** argv)
{
	int test_case;
	int T;
	cin>>T;

	for(test_case = 1; test_case <= T; ++test_case)
	{
        int a, b;
        cin >> a;
        cin >> b;
        if (a == b) {
            cout<< '#' << test_case << " =" << endl;
        } else if ( a > b ) {
            cout << '#' << test_case << " >" << endl;
        } else if (a < b ) {
            cout << '#' << test_case << " <" << endl;
        }
	}
	return 0;//정상종료시 반드시 0을 리턴해야합니다.
}
```



**algoholic 코드**

함수를 분할하였으며, 삼항 if문을 사용했다.

```c++
#include <iostream>
 
using namespace std;
 
char solve() {
    int a, b;
    cin >> a >> b;
    if (a == b)
        return '=';
    else
        return a > b ? '>' : '<';
}
 
int main()
{
    int t;
    cin >> t;
    for (int tc = 1; tc <= t; tc++) {
        cout << "#" << tc << " " << solve() << "\n";
    }
}
```



---

### 백만장자



**내 1차**

```c++
/*
3
3
10 7 6
3
3 5 9
5
1 1 3 1 2
*/
#include <iostream>
using namespace std;

int find_max(int start, int end, int array[])
{
    int max_idx = start;
    int max_num = array[start];
    int i;
    for (i=start; i<end; i++) {
        if (array[i] > max_num) {
            max_num = array[i];
            max_idx = i;
        }
    }
    return max_idx;
}

int sell_product(int now, int max_idx, int array[])
{
    int profit = 0;
    int i;
    for (i=now; i < max_idx; i++){
        profit += array[max_idx] - array[i];
    }
    return profit;
}

int main()
{
    ios::sync_with_stdio(0);
	cin.tie(0);
    int testcase;
    cin >> testcase;
    for (int tc=1; tc <= testcase; tc++){
        int N;
        cin >> N;
        int array[N];
        int i;
        for (int i = 0; i < N; i++){
            cin >> array[i];
        }
        int z = 0;
        int ans = 0;
        while (z < N){
            int next_max_idx = find_max(z, N, array);
            int new_profit = sell_product(z, next_max_idx, array);
            ans += new_profit;
            z = next_max_idx + 1;
        }
        cout << '#' << tc << " " << ans << endl;
    }
}
```





**95kms**

```c++
#include<stdio.h>
#include<stdlib.h>
int main()
{
    int T;
    scanf("%d",&T);
    for(int i=0;i<T;++i)
    {
        int N;
        int buy=0;
        int buycount=0;
        long long result=0;
        scanf("%d",&N);
        int* arr=(int*)calloc(sizeof(int),N);
        for(int i=0;i<N;++i)
        {
            scanf("%d",&arr[i]);
        }
        int count=0;
        int max=0;
        while(count<=N-1)
        {
            buycount=0;
            for(int i=count;i<N;++i)
            {
                if(arr[i]>arr[max])
                    max=i;
            }
            int sum=0;
            for(int i=count;i<max;++i)
            {
                sum+=arr[i];
            }
            buycount=max-count;
            result+=buycount*arr[max]-sum;
            count=max+1;
            ++max;  
        }
        printf("#%d %lld\n",i+1,result);
        free(arr);
    }
    return 0;
}
```



**열심히살아요**

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
long *NUM, T;
long find();
long MAX(long pos);
 
int main() {
    int T;
    cin >> T;
    for (int i = 1; i <= T; i++) {
        cout << "#" << i << " " << find() << endl;
    }
}
 
long find() {
    long answer = 0;
    cin >> T;
    NUM = new long[T];
    int index = 0;
    for (long i = 0; i < T; i++) {
        cin >> NUM[i];
    }
    long price = 0;
    long count = 0;
    for (long i = 0; i < T; i++) {
        index = MAX(i);
        for (long next = i; next < index; next++) {
            price += NUM[next];
            count++;
        }
        i = index;
        answer += NUM[index] * count - price;
        count = 0; price = 0;
    }
    return answer;
}
 
long MAX(long pos) {
    long M = NUM[pos], index = pos;
    for (long i = pos; i < T; i++) {
        if (M < NUM[i]) { M = NUM[i]; index = i; }
    }
    return index;
}
```

