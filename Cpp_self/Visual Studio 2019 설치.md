# Visual Studio 2019 설치

MS 에서 난 분명 2017을 설치하려고 했는데,

header 부분 읽어보니 2019년 이더라...



일단 데스크톱 체크 눌렀고 나머진 기본값으로 설치

6.5GB 정도 공간이 필요했다.



옵션 선택

나는 개발환경은 일반으로 맞추고, 테마는 어둡게 했다.

그리고 한양메일로 로그인 했다.



나중에 installer로 옵션을 변경할 수 있는지 조사해야 한다.





### 새 프로젝트 생성

Visual Studio 2019를 들어가면 여러가지 시작 옵션이 있다.

내가 본 유튜브는 코드없이 시작하기 누름.

<https://www.youtube.com/watch?v=hKYNA1SiCFM>

파일 - 새 프로젝트 - 프로젝트 - 빈프로젝트 - 생성



Hello World 출력해보자

디버그하지 않고 실행 (`Ctrl + F5`)

```c++
#include <stdio.h>

main()
{
	printf("Hello World!\n");
	printf("I am C++");
}

```

실행결과 4번째 줄에서 error



그래서 고침

```c++
#include <stdio.h>

int main()
{
	printf("Hello World!\n");
	printf("I am C++");
	return 0;
}
```

명령 프로토콤 화면이 뜨면서 문자가 출력됨을 볼 수 있었다.



하지만 위는 c언어 스타일이지 c++은 아니다.

```c++
#include <iostream>

int main()
{
	std::cout << "Hello C++\n";
	return 0;
}
```

창이 하나 뜨고, c++ 파일이 실행파일로 만들어져서 실행 되었음을 알 수 있었다.



다른 스타일로 출력 

```c++
#include <iostream>

using namespace std;

int main()
{
	cout << "Hello C++" << endl;
	return 0;
}
```





왜 int main() 이여야 하는지 조사해야 한다.

visual studio 를 어떻게 더 잘 쓰는지 팁을 알아봐야 한다.

