# 30장 Date

## 30.1 Date 생성자 함수

- `Date`는 생성자 함수
- `Date` 생성자 함수로 생성한 `Date` 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.

### 30.1.1 new Date()

- `Date` 생성자 함수를 인수 없이 `new`연산자와 함께 호출하면 현재 날짜와 시간을 가지는 `Date` 객체를 반환

```jsx
new Date(); // Sat Oct 19 2024 17:27:23 GMT+0900 (한국 표준시)

// new 연산자 없이 호출 → 날짜와 시간 정보를 나타내는 문자열 반환
Date(); // 'Sat Oct 19 2024 17:28:22 GMT+0900 (한국 표준시)'
```

## 30.2 Date 메서드

### 30.2.1 Date.now

- 현재 시간까지 경과한 밀리초를 숫자로 반환

```jsx
const now = Date.now(); // → 1729327705079
new Date(now); // Sat Oct 19 2024 17:47:58 GMT+0900 (한국 표준시)
```

### 30.2.4 Date.prototype.getFullYear

- `Date` 객체의 연도를 나타내는 정수를 반환

### 30.2.5 Date.prototype.setFullYear

- `Date` 객체에 연도를 나타내는 정수를 설정
- 연도 이외에 옵션으로 월,일도 설정 가능

```jsx
const today = new Date();

// 연도 지정
today.setFullYear(2000);
today.getFullYear(); // 2000

// 연도/월/일 지정
today.setFullYear(2002, 02, 16);
today.getFullYear(); // 2002
```

### 30.2.6 Date.prototype.getMonth

- `Date` 객체의 월을 나타내는 0~11의 정수를 반환

### 30.2.7 Date.prototype.setMonth

- `Date` 객체에 월을 나타내는 0~11의 정수를 설정

```jsx
const today = new Date();

// 월 지정
today.setMonth(0); // 1월
today.getMonth(); // 0

// 월/일 지정
today.setMonth(2, 14); // 3월 14일
today.getMonth(); // 2
```

### 30.2.8 Date.prototype.getDate

- `Date` 객체의 날짜 (1~31)를 나타내는 정수를 반환

### 30.2.9 Date.prototype.setDate

- `Date` 객체에 날짜(1~31)를 나타내는 정수를 설정

```jsx
const today = new Date();

// 날짜 지정
today.setDate(8);
today.getDate(); // 8
```

### 30.2.10 Date.prototype.getDay

- `Date` 객체의 요일을 나타내는 정수를 반환
- 일요일 → 0 ~ 토요일 → 6

```jsx
new Date('2024/10/19').getDay(); // 6
```

### 30.2.11 Date.prototype.getHours

- `Date` 객체의 시간(0~23)을 나타내는 정수를 반환

### 30.2.12 Date.prototype.setHours

- `Date` 객체에 시간(0~23)을 나타내는 정수를 설정
- 시간 이외에 옵션으로 분, 초, 밀리초도 설정 가능

```jsx
const today = new Date();

// 시간 지정
today.setHours(9);
today.getHours(); // 9

// 시간/분/초/밀리초 지정
today.setHours(0, 0, 0, 0);
today.getHours(); // 0
```

### 30.2.19 Date.prototype.getTime

- 1979년 1월 1일 00:00:00(UTC)를 기점으로 `Date` 객체의 시간까지 경과된 밀리초를 반환

```jsx
new Date('2020/07/24/12:30').getTime(); // -> 1595561400000
```

### 30.2.22 Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 `Date` 객체의 날짜를 반환

### 30.2.23 Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 `Date` 객체의 시간을 표현한 문자열을 반환

### 30.2.24 Date.prototype.**toISOString**

- ISO 8610 형식으로 `Date` 객체의 날짜와 시간을 표현한 문자열을 반환

```jsx
const today = new Date('2018/4/22/15:00');

today.toString();     // 'Mon Apr 22 2018 15:00:00 GMT+0900 (한국 표준시)'
today.toDateString(); // 'Mon Apr 22 2018'
today.toTimeString(); // '15:00:00 GMT+0900 (한국 표준시)'

today.toISOString();  // '2018-04-22T06:00:00.000Z'
today.toISOString().slice(0, 10); // '2018-04-22'
today.toISOString().slice(0, 10).replace(/-/g, ""); // '20180422'
```

### 30.2.25 **Date.prototype.toLocaleString**

- 인수로 전달한 로캘을 기준으로 `Date` 객체의 날짜와 시간을 표현한 문자열을 반환
- 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로켈을 적용

### 30.2.26 Date.prototype.toLocalTimeString

- 인수로 전달한 로캘을 기준으로 `Date` 객체의 시간을 표현한 문자열을 반환
- 인수를 생략한 경우 브라우저가 동작 중인 시스템의 로켈을 적용

```jsx
const today = new Date('2019/1/1/00:00');

today.toString();     // 'Tue Jan 01 2019 00:00:00 GMT+0900 (한국 표준시)'

// toLocaleString
today.toLocaleString(); // '2019. 1. 1. 오전 12:00:00'
today.toLocaleString('ko-KR'); // '2019. 1. 1. 오전 12:00:00'
today.toLocaleString('en-US'); // '1/1/2019, 12:00:00 AM'
today.toLocaleString('ja-JP'); // '2019/1/1 0:00:00'

// toLocaleTimeString
today.toLocaleTimeString();  // '오전 12:00:00'
today.toLocaleTimeString('ko-KR'); // '오전 12:00:00'
today.toLocaleTimeString('en-US'); // '12:00:00 AM'
today.toLocaleTimeString('ja-JP'); // '0:00:00'
```

## 30.3 Date를 활용한 시계 예제

```jsx
(function printNow() {
	const today = new Date();

    const dayNames = [
    	'(일요일)',
        '(월요일)',
        '(화요일)',
        '(수요일)',
        '(목요일)',
        '(금요일)',
        '(토요일)'
    ];
//getDay 메서드는 해당 요일을 나타내는 정수를 반환한다const day = dayNames[today.getDay()];

    const year = today.getFullYear();
    const month = today.getMonth() + 1;
    const date = today.getDate();
    const hour = today.getHours();
    const minute = today.getMinutes();
    const second = today.getSeconds();
    const ampm = hour >= 12? 'PM' : 'AM';

//12시간제로 변경
    hour %=12;
    hour = hour || 12;

//10미만인 분과 초를 2자리로 변경
    minute = minute < 10? '0' + minute : miniute;
    second = second < 10? '0' + second : second;

    const now = `${year}년 ${month}월 ${date}일 ${day} ${hour} : ${minute}:${second} ${ampm}`;
    console.log(now);

//1초마다 printNow 함수를 재귀 호출한다.setTimeout(printNow, 1000);
}());
```
