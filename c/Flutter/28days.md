# 28일간의 플루터

## 1일차
1. 다른 프로그래밍 언어 바이너리와는 다르게 `/Users/jaewon/flutter` 처럼 특정 폴더에 설치되지 않고 다운로드 한 위치로부터 PATH 을 설정하게 된다. 나는 `/Users/jaewon/flutter`로 설정하였다. (flutter 폴더 안에 .git 파일이 포함되어 있어야 했다. 업데이트를 위한 것 같다) 

2. Dart 관련
    - 코드들은 다 main 함수를 가지고 있다.
    - `var` 키워드를 이용해서 변수를 선언할 수 있다. 상수는 `const` 키워드를 이용한다. 기본적으로 Dart는 타입이 있는 정적언어다. 하지만 타입 추론 기능이 있어서 타입을 명시하지 않아도 된다.
      - 타입을 명시하는 경우에는 `[int, String ..] 변수명` 으로 명시 할 수 있다.
    - if, while, for 문법은 자바스크립트와 동일하다.
    - 함수의 경우 C언어와 비슷하게 사용 할 수 있다. 물론 자바스크립트에서의 화살표 함수도 가능하다.

## 2일차
### 다트 데이터 타입
```dart
void main() {
  // dart에는 5가지의 기본 타입이 있다.
  // int, double, String, bool, dynamic
  int a = 10;
  var a = 10; // 자동으로 타입을 추론한다. (auto 같은 키워드)

  String b = "Hello World";
  String b = 'Hello World';
  String b = r'Hello\nWorld'; // raw string
  String b = '''
    Hello World
  '''
  String b = """
    Hello World
  """

  bool c = true;
  bool c = false;

  // dynamic 타입에는 모든 타입의 값들이 들어갈 수 있다.
  dynamic d = 10;
  d = "Hello World";
}
```

### 다트 타입 변환
```dart
// dart에서도 모든 것이 오브젝트이다.
void main() {
  int.parse('1'); // int 오브젝트의 parse 메소드를 이용, string -> int
  double.parse('1.1');

  1.toString(); // int 오브젝트의 toString 메소드
  1.1.toString();
}
```

### 상수와 null
```dart
// var처럼 타입이 추론된다.
const a = 10;
const b = "Hello World";
const c = false;

const a int = 10; // 처럼 타입 명시도 가능하다.

print(a.runtimeType + b.runtimeType + c.runtimeType)
// 타입 출력이 가능하다.

int num; // num은 null로 초기화된다.
print(num);
```

### 반복문
```dart
void main() {
  for(var i = 0; i < 10; i++) {} // 정석 for loop

  var numbers = [1, 2, 3, 4];

  for (var number in numbers) { // for-in loop
    print(number);
  }

  numbers.forEach(n => print(n));


  while (true) {
    
  }

  do {

  } while (true)
}
```