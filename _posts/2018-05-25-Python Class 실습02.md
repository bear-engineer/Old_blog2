---
layout: post
title:  "Python Class 실습02"
date:   2018-05-25
excerpt: "Python Class"
tag:
- python
---
### 도서관리 시스템
- 출제 문제

```
도서 관리 프로그램
    Library, Book, User클래스 구현
        프로그램 시작시 도서 5권 정도를 가진 상태로 시작

    Library
        attrs
            name: 도서관명
            book_list: 도서 목록 (Book인스턴스의 목록)
        methods
            add_book
            remove_book
        property
            info: 가지고 있는 도서 목록을 보기좋은 텍스트로 출력 (빌려간 도서는 출력 안해도 됨)

    Book
        attrs
            title: 제목
            location: 현재 자신이 어떤 Library 또는 User에게 있는지를 출력
        property
            is_borrowed: 대출되었는지 (location이 User인지 Library인지 확인)

    User
        attrs
            name: 이름
            book_list: 가지고 있는 도서 목록
        methods
            borrow_book(library, book_name): library로부터 book을 가져옴
            return_book(library, book_name): library에 book을 다시 건네줌
```
### 풀이
- 책을 도서관이 가지고 있어야하는데, 책이 하나하나가 객채다. 즉 처음부터 도서관이 가지고 있을수 없다.

- 먼저 `Library`, `Book`, `User` 클래스를 지정해주고 `Book` 에 책 객채를 만들어 넣어주자, 이때 책의 `location` 을 `library` 라고 미리 설정해두자.

- 그리고 `Library` 의 `book_list` 에 책 객채들을 넣어준다.

- `User` 가 책을 대여할때 도서관의 `book_list` 에서 해당 책을 지워주고 `User` 의 `book_list` 에 넣어준다. `location` 을 `user` 로 바꾸어준다.



```python
class Library():
    def __init__(self, name):
        """
        도서관
        """
        self.name = name
        self.book_list = []

    def add_book(self, book):
        """
        도서 구입
        """
        if book.title not in self.book_list:
            self.book_list.append(book.title)
            print(f'도서 총판에서 {book.title} 책을 구입하여 배치하였다.')
        else:
            print(f'{book.title} 은/는 중복 구입한 책이다.')

    def remove_book(self, book):
        """
        도서 소각
        """
        if book.title in self.book_list:
            self.book_list.remove(book.title)
            print(f'{book.title} 책을 소각하였습니다.')
        else:
            print(f'{book.title} 이라는 책은 없습니다.')

    @property
    def info(self):
        """
        도서관 소장도서 목록 출력
        """
        if len(self.book_list) > 0:
            print(f'-{self.name} 소장 도서목록')

            for index, book in enumerate(self.book_list):
                index += 1
                print(f'{index}. {book}')
        else:
            print('세상에 도서관에 책이 없을리가...?')

class Book:
    def __init__(self, title):
        """
        도서 총판
        """
        self.title = title
        self.location = 'Libary'

    @property
    def info(self):
        if self.title == '':
            print('없는 책입니다.')
        else:
            print(f'book Name: {self.title}')

    @property
    def is_borrowed(self):
        """
        도서 대여 여부 및 위치확인
        """
        if self.location == 'Libary':
            print(f'{self.title} 은 현재 도서관에 있습니다.')
        else:
            print(f'{self.title} 은 현재 대여중입니다.')

class User:
    def __init__(self,name):
        """
        사용자
        """
        self.name = name
        self.book_list = []

    def borrow_book(self, library, book):
        """
        도서 대여
        """
        if book.title in library.book_list:
            library.book_list.remove(book.title)
            self.book_list.append(book.title)
            book.location = self.name
            print(f'{book.title}을 {self.name}님이 대여하였습니다.')
            print(f'책은 현재 {self.name}님이 대여하신 상태입니다.')
        else:
            print(f'{book.title} 은 대여중이거나 {library.name}에 소장중이지 않습니다.')

    def return_book(self, library, book):
        """
        도서 반납
        """
        if book.title in self.book_list:
            self.book_list.remove(book.title)
            library.book_list.append(book.title)
            book.location = library.name
            print(f'{book.title}을 {self.name}님이 반납하셨습니다.')
            print(f'{book.title}은 {library.name}에 소재중입니다.')
        else:
            print(f'{book.title}은 {self.name}님이 빌리지 않았거나, 잘못된 호출입니다.')

    @property
    def info(self):
        """
        유저의 도서대여 상황
        """
        if len(self.book_list) > 0:
            print(f'-{self.name} 님의 도서대여 목록')
            for index, book in enumerate(self.book_list):
                index += 1
                print(f'{index}. {book}')
        else:
            print(f'{self.name}님은 현재 대여중인 책이 없습니다.')
```

### 출력 결과
```python
gs_library = Library('관산도서관')
```


```python
b_01 = Book('도와줘요! 한영에몽!')
b_02 = Book('망하면 처음부터 다시. CSS')
b_03 = Book('치질걸리는게 빠를까? 코딩을 다하는게 빠를까?')
b_04 = Book('고질적 문제적 당신, 영타')
b_05 = Book('싶프트 눌러요 빨리!, Python Class Naming')
b_06 = Book('다필요없어! 웰컴! 퐈이쒄')
b_07 = Book('오지게 빠른 python')
b_08 = Book('당신도 할수있다 "멘붕"')
b_09 = Book('미움받을 용기')
b_10 = Book('갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"')
b_11 = Book('님 안사요')
```


```python
gs_library.add_book(b_01)
gs_library.add_book(b_02)
gs_library.add_book(b_03)
gs_library.add_book(b_04)
gs_library.add_book(b_05)
gs_library.add_book(b_06)
gs_library.add_book(b_07)
gs_library.add_book(b_08)
gs_library.add_book(b_09)
gs_library.add_book(b_10)
gs_library.add_book(b_11)
```

    도서 총판에서 도와줘요! 한영에몽! 책을 구입하여 배치하였다.
    도서 총판에서 망하면 처음부터 다시. CSS 책을 구입하여 배치하였다.
    도서 총판에서 치질걸리는게 빠를까? 코딩을 다하는게 빠를까? 책을 구입하여 배치하였다.
    도서 총판에서 고질적 문제적 당신, 영타 책을 구입하여 배치하였다.
    도서 총판에서 싶프트 눌러요 빨리!, Python Class Naming 책을 구입하여 배치하였다.
    도서 총판에서 다필요없어! 웰컴! 퐈이쒄 책을 구입하여 배치하였다.
    도서 총판에서 오지게 빠른 python 책을 구입하여 배치하였다.
    도서 총판에서 당신도 할수있다 "멘붕" 책을 구입하여 배치하였다.
    도서 총판에서 미움받을 용기 책을 구입하여 배치하였다.
    도서 총판에서 갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝" 책을 구입하여 배치하였다.
    도서 총판에서 님 안사요 책을 구입하여 배치하였다.



```python
gs_library.info
```

    -관산도서관 소장 도서목록
    1. 도와줘요! 한영에몽!
    2. 망하면 처음부터 다시. CSS
    3. 치질걸리는게 빠를까? 코딩을 다하는게 빠를까?
    4. 고질적 문제적 당신, 영타
    5. 싶프트 눌러요 빨리!, Python Class Naming
    6. 다필요없어! 웰컴! 퐈이쒄
    7. 오지게 빠른 python
    8. 당신도 할수있다 "멘붕"
    9. 미움받을 용기
    10. 갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"
    11. 님 안사요



```python
gs_library.remove_book(b_11)
```

    님 안사요 책을 소각하였습니다.



```python
gs_library.info
```

    -관산도서관 소장 도서목록
    1. 도와줘요! 한영에몽!
    2. 망하면 처음부터 다시. CSS
    3. 치질걸리는게 빠를까? 코딩을 다하는게 빠를까?
    4. 고질적 문제적 당신, 영타
    5. 싶프트 눌러요 빨리!, Python Class Naming
    6. 다필요없어! 웰컴! 퐈이쒄
    7. 오지게 빠른 python
    8. 당신도 할수있다 "멘붕"
    9. 미움받을 용기
    10. 갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"



```python
b_01.info
b_02.info
b_11.info
```

    book Name: 도와줘요! 한영에몽!
    book Name: 망하면 처음부터 다시. CSS
    book Name: 님 안사요



```python
youn = User('세현')
```


```python
youn.borrow_book(gs_library, b_10)
```

    갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"을 세현님이 대여하였습니다.
    책은 현재 세현님이 대여하신 상태입니다.



```python
youn.info
```

    -세현 님의 도서대여 목록
    1. 갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"



```python
b_10.is_borrowed
```

    갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝" 은 현재 대여중입니다.



```python
b_01.is_borrowed
```

    도와줘요! 한영에몽! 은 현재 도서관에 있습니다.



```python
youn.return_book(gs_library, b_10)
```

    갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"을 세현님이 반납하셨습니다.
    갑자기 분위기 싸하게 만드는 당신의 한마디, "잇힝"은 관산도서관에 소재중입니다.



```python
youn.info
```

    세현님은 현재 대여중인 책이 없습니다.
