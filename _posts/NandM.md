# N과 M

백준에 N과 M이라는 시리즈가 있다. 총 12문제인데 순열조합 문제이다. 부르트 포스로 풀어보자

## 1번

문제
자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
입력
첫째 줄에 자연수 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

출력
한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

예제 입력 1 
3 1
예제 출력 1 
1
2
3

### 풀이

결국 모든 순열 NPM을 출력하라는 건데
재귀호출을 통한 완전탐색이 가장 먼저 생각난다.
기저 조건은 M개를 모두 카운트 했을 때 이고, 이 조건에 들어갈 경우 순열을 출력해 준다.
subcase는 선택하지 않은 수들 중에서 하나의 수를 선택하는 것이다.
재귀호출이 포함된 코드 앞뒤로 넣었다가 빼는 코드를 추가한다.

이를 구현하면,

``` c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> v;
bool chosen[9] = {false};

void permutation(int M, int N)
{
	if(N == 0) {
		for(vector<int>::iterator iter = v.begin(); iter < v.end(); ++iter) {
			cout << *iter;
			if(iter + 1 != v.end())
				cout << ' ';
		}
		cout << endl;
		return;
	}

	for(int i = 1; i < M + 1; ++i) {
		if(!chosen[i]) {
			chosen[i] = true;
			v.push_back(i);
			permutation(M, N - 1);
			chosen[i] = false;
			v.pop_back();
		}
	}

	return;
}

int main()
{
	int M, N;

	cin >> M >> N;
	permutation(M, N);
	
	return 0;
}
```

## 2번

문제
자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
고른 수열은 오름차순이어야 한다.
입력
첫째 줄에 자연수 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

출력
한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

예제 입력 1
3 1
예제 출력 1
1
2
3
예제 입력 2
4 2
예제 출력 2
1 2
1 3
1 4
2 3
2 4
3 4

### 풀이

요컨데 앞의 문제와의 유일한 차이점은 첫 번째 수보다 작은 수가 그 뒤에 나와서는 안된다는 점이다.
algospot의 튜토리얼과 종만북에서 재귀호출 문제들을 풀면서 배운 아주 효과적인 방법은 index를 제한하는 것인 이 경우 main으로 돌아가는 for 루프문 전에 index를 설정하는 for 루프문을 추가하는 것이 좋아보인다.
index의 최대값부터 1씩 감소하면서 처음으로 true값을 만나는 지점을 찾아 index로 설정하여 다음 for 루프로 넘겨준다. 그 외에는 첫 번째 문제와 같다.데


``` c++
#include <iostream>
#include <vector>
using namespace std;

vector<int> v;
bool chosen[9] = {false};

void combination(int N, int M)
{
	if(M == 0) {
		for(vector<int>::iterator iter = v.begin(); iter < v.end(); ++iter)
			cout << *iter << ' ';
		cout << "\n";
		return;
	}

	int index;
	for(index = 8; index > 0; --index) {
		if(chosen[index]) break;
	}

	for(int i = index + 1; i < N + 1; ++i) {
		chosen[i] = true;
		v.push_back(i);
		combination(N, M - 1);
		chosen[i] = false;
		v.pop_back();
	}
	return;
}

int main()
{
	int N, M;
	cin >> N >> M;
	combination(N, M);

	return 0;
}
```
