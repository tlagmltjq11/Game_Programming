## 🔔 QuadTree

![쿼드트리](https://user-images.githubusercontent.com/43705434/128810882-d7070798-8d4f-40aa-92d0-2edac1504c6b.PNG)<br>
<br>


**트리 자료구조 중 하나로 자식 노드를 4개씩 갖고있는 트리를 의미한다.**<br>
지형 관리 기법으로 자주 사용된다. 일단 n차원 공간을 4등분 한후, 분할된 공간을 다시 필요에 따라 4등분하고...<br>
더 이상 분할이 필요하지 않은 시점 까지 반복하다보면 위 그림과 같은 모양이 된다.<br>
즉 복잡하게 표현해야 하는 곳은 세밀하게 분할되고 그렇지 않은 곳은 큰 그룹으로 남는다.<br>
⭐ **이미지 용량, 맵, 충돌, 컬링, LOD 등 다양한 곳에서 최적화 기법으로 사용된다.** ⭐<br>
<br>
<br>

👉 **대표적인 문제 : 흑백 이미지를 간단하게 압축**<br>

![흑백이미지1](https://user-images.githubusercontent.com/43705434/128810884-69ba15dc-4147-44d8-8b4e-70531ace5b5b.PNG)<br>
![흑백이미지2](https://user-images.githubusercontent.com/43705434/128810890-fcf3d62a-5686-43cf-a24a-cbfff2f6d8f6.PNG)<br>
<br>

같은 숫자로 구성되어 있다면 하나로 묶어버리고 그렇지 않으면 다시 4개의 구역으로 나누는 형태<br>
압축된 데이터 : (0(1101)1((0011)(0111)1(1110))<br>

쿼드트리 분할 알고리즘은 [백준 1992번](https://github.com/tlagmltjq11/Algorithm/blob/master/Divide%20and%20Conquer/1992.cs)  문제를 풀며 적용해볼 수 있다.<br>
<br>
<br>

👉 **쿼드트리 응용**<br>

* 길찾기<br>
맵에서 길찾기를 시킬때, 쿼드트리를 이용해 최적화 할 수 있다.<br>
일반적으로는 그냥 타일 맵에서 길찾기를 시키는데, 그러면 맵 크기가 커지면 커질수록 탐색해야 하는 타일 개수가 늘어나 부하가 커진다.<br>
하지만 쿼드트리는 복잡하게 표현할 필요가 없는 곳들은 큰 네모 하나로 묶어서 표현하기 때문에 부하를 상대적으로 줄일 수 있다.<br>
-> **큰 덩어리 단위로 필요없는 데이터를 제거함으로써, 처리해야할 데이터량을 빠르게 줄일 수 있다.** ⭐⭐<br>

* 컬링<br>
정석적으로 컬링하는 것보다 쿼드 트리라는 자료구조를 이용하여 컬링하는 쪽이 연산의 횟수가 적어져 빠르기 때문이다<br>
**필요 없는 데이터를 큰 덩어리 단위로 버릴 수 있게 되므로 거대한 지형을 빠르게 검색할 수 있게 된다.** ⭐⭐<br>
짧게 말하면 프로스텀이 형성된 지역을 제외하곤 모두 그리지 않는다. 프로스텀이 형성된 지역만 그린다.<br>
쿼드트리로 4등분을 또 4등분... 4등분.. 했으니까.. 프로스텀에 걸리지 않는 영역은<br>
부모노드 통째로 제외시키고 만약에 부분만 걸렸다 하면. 그 노드의 4등분 된것을 또 판단하여<br>
프로스텀에 걸리지 않으면 제외시킨다.. 이렇게 프로스텀에 부분적으로 걸려있으면 작게작게<br>
들어가서 프로스텀에 걸리는 지역만 렌더링 한다.<br>

* 충돌<br>
![쿼드트리충돌](https://user-images.githubusercontent.com/43705434/128810894-848215be-054a-431a-aa01-72fac46ebb93.PNG)<br>

<br>
<br>

## BSP 알고리즘
로그라이크 류 같은 게임에서 플레이 때 마다 맵이 랜덤하게 바뀌는 경우가 있는데 해당 시스템을 구현하는데 좋은 포스팅이 있다.<br>
https://blog.naver.com/jh20s/222343029141 <br>
<br>
<br>

## 참고링크
LOD<br>
https://wonjayk.tistory.com/192?category=535169<br>
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=nowcome&logNo=130031941858<br>
<br>

https://copynull.tistory.com/308 (쿼드트리를 사용한 오클루전 컬링 구현)<br>
https://chessire.tistory.com/entry/%EC%BF%BC%EB%93%9C%ED%8A%B8%EB%A6%ACQuad-tree <br>
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=daroobil&logNo=110160605 <br>
