# 선분 교차 판정
[CCW](https://github.com/r3j0/TIL/blob/main/Algorithm/algo_20251017_ccw.md?plain=1) 를 활용해 두 선분이 교차하는지 판정할 수 있다.

<img width="880" height="468" alt="image" src="https://github.com/user-attachments/assets/f8b55ea2-5c7a-4391-abe9-837df157c9c2" />

주요 아이디어는 다음과 같다. 한 선분을 기준으로 다른 선분의 양 끝점이 각자 다른 방향에 있는지를 CCW를 활용해 확인하는 것이 포인트다. 하지만 이 조건만으로는 선분 교차를 완전히 판정할 수 없다.

<img width="702" height="432" alt="image" src="https://github.com/user-attachments/assets/ea9fa46e-d30e-4843-8a0f-d24d24aa2500" />

두 선분이 교차하지 않지만 다른 선분의 양 끝점이 각자 다른 방향에 있을 수 있다. 이를 해결하기 위해, **다른 선분을 기준으로 잡아서 한번 더 동일한 조건을 판정해야 한다.**

<img width="701" height="599" alt="image" src="https://github.com/user-attachments/assets/40ff8e43-5ae5-4ca7-83e2-292f1f73d966" />

`CCW(A, B, C)` 와 `CCW(A, B, D)` 가 다른 방향을 가리켜야 하고, `CCW(C, D, A)` 와 `CCW(C, D, B)` 가 다른 방향을 가리켜야 한다.

두 선분이 접하는 경우 또한 알아보자.

<img width="623" height="656" alt="image" src="https://github.com/user-attachments/assets/2496761c-077c-4f99-8b62-5f5ab85c2723" />

한 선분을 기준으로 다른 선분 양 끝점에 대한 CCW 가 다른 방향이거나 0일 때 선분이 교차한다는 것을 판정하기 위해, 다음과 같이 조건문을 작성할 수 있다.
```
CCW(A, B, C) * CCW(A, B, D) <= 0 && CCW(C, D, A) * CCW(C, D, B) <= 0
```

여기서 고려해야 할 예외가 하나 더 존재한다.

<img width="644" height="602" alt="image" src="https://github.com/user-attachments/assets/f5cf9ffd-c3e2-4792-8cff-2e484dbf870a" />

CCW가 모두 0이지만 선분이 교차하지 않는 경우이다. **이를 해결하기 위해, `A <= D && C <= B` 에 대한 조건도 판정해야 한다.**

전체를 정리해서 코드로 작성해보았다. 참고 : [https://kimwooil.tistory.com/466](https://kimwooil.tistory.com/466)

### Python 
```py
# CCW
def CCW(p1, p2, p3):
    # 벡터 AB (점 B - 점 A) X 벡터 BC (점 C - 점 B)
    x1, y1 = p1
    x2, y2 = p2
    x3, y3 = p3
    return ((x2-x1)*(y3-y2))-((x3-x2)*(y2-y1))

# 선분 교차
def lineCross(p1, p2, p3, p4):
    # 점 정렬
    if p1 > p2: p1, p2 = p2, p1
    if p3 > p4: p3, p4 = p4, p3

    # 두 선분을 기준으로 한 CCW
    res1 = CCW(p1, p2, p3) * CCW(p1, p2, p4)
    res2 = CCW(p3, p4, p1) * CCW(p3, p4, p2)

    # 예외. 두 선분이 같은 직선 상에 있을 때 겹치는 판정
    if res1 == res2 == 0: 
        return (p1 <= p4) and (p3 <= p2)
    
    # 두 선분이 교차하는가에 대한 판정
    return res1 <= 0 and res2 <= 0
```
