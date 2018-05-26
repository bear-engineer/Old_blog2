---
layout: post
title:  "Python Class"
date:   2018-05-23
excerpt: "Python Class"
tag:
- python
---
### Class
클래스는 객체를 만들기 위한 틀이다.

```python
# 클래스명은 맨 앞은 대문자가 와야한다.
class Shop:
  # __init__는 이 클래스의 초기화 메소드
  def __init__(self, name):
    self.name = name
```

```python
# 위의 초기화 함수에서 self는 여기서는 `lotteria`를 가르킨다. 위의 name는 위치인자와 같이 `롯데리아`의 값을 가진다.
# 키워드 인자로 지정 할 수도 있다.
lotteria = Shop('롯데리아')
```

### Class 실습 01

```python
# 패스트 캠퍼스
class Fc_school:
    def __init__(self, name, mental,spot):
        self.name = name
        self.mental = mental
        self.spot = spot

    def show_info(self):
        print(f'이름 : {self.name}, 정신력 : {self.mental}, 직위 : {self.spot}')
# 공부
class Study(Fc_school):
    def __init__(self, name, mental, spot):
        super().__init__(name, mental, spot)
    # 학생정보 확인
    def study_python(self,x):
        x = -10
        print(f'{self.name}님의 정신력이 {x}만큼 감소하였습니다.')
        self.mental += x
# 강의
class Lecture(Fc_school):
    def __init__(self, name,mental,spot,fast):
        super().__init__(name, mental, spot)
        self.fast = fast
    # 강사정보 확인
    def show_info(self):
        print(f'이름 : {self.name}, 정신력 : {self.mental}, 직위 : {self.spot}, 빠름 : {self.fast}')
    # 강의 항의
    def fast_horse(self, name):
        self.fast += name.mental
        print(f'{name.name}님이 너무 빠르다며 항의합니다. {self.name}님의 속도가{name.name}님의 정신력 {name.mental}만큼 더 빨라집니다.')

sh = Study('세현',100,'수강생')
sh.show_info()

이름 : 세현, 정신력 : 100, 직위 : 수강생

sh.study_python(10)

세현님의 정신력이 -10만큼 감소하였습니다.

sh.show_info()

이름 : 세현, 정신력 : 90, 직위 : 수강생

lhy = Lecture('한영',1000,'강사',999999)
lhy.show_info()

이름 : 한영, 정신력 : 1000, 직위 : 강사, 빠름 : 999999

lhy.fast_horse(sh)

세현님이 너무 빠르다며 항의합니다. 한영님의 속도가 세현님의 정신력 90만큼 더 빨라집니다.

lhy.show_info()

이름 : 한영, 정신력 : 1000, 직위 : 강사, 빠름 : 1000089
```
