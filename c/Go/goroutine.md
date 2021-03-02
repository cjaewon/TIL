## 고루틴 정리

### channel과 range
```go
for c := range channels {
  fmt.Println(c)
}
```
`range 채널` 을 통해서 지속적으로 수신할 수 있다. 이를 통해서 워커 풀을 구현 할 수 있는데 다음과 같다.

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	jobs := make(chan int)

	go worker(1, jobs)
	go worker(2, jobs)
	go worker(3, jobs)

	for i := 0; i < 100; i++ {
		jobs <- i 
	}

	var a string
	fmt.Scan(&a)
}

func worker(id int, jobs <-chan int) {
	for job := range jobs { // range jobs 를 통해 채널을 통해 수신을 받음
		fmt.Println(id, job)
	}
}
```
위 코드에서는 고루틴 3개 (워커 3개)를 돌리고 있다. 송신 된 채널은 생성된 워커들 중에서 가장 먼저 수신 할 수 있는 쪽이 해당 값을 가져간다. 그럼 그 외 워커들에게는 채널이 전달이 되지 않는다. 

<!-- 버퍼를 사용하면 한개 한개식 송신하는 위 경우와 다르게 여러개의 버퍼값들을 가져와서 해당 고루틴(워커)로 실행한다.
버퍼를 사용 또는 사용하지 않더라도 3개의 고루틴(워커)들이 작업을 나누어서 진행한다. (확인 할 수 없음) -->

