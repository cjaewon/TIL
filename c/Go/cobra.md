# Cobra 모듈 사용법 및 설계
> [Cobra](https://github.com/spf13/cobra)는 CLI 도구를 고 언어에서 개발하도록 도와주는 라이브러리이다.  
대표적으로 github cli, hugo 등이 사용 중이다.

## 시작하기
Cobra를 사용하면 각각 명령어를 레고처럼 쌓아올려서 CLI 을 만들 수 있다.

### 기본 설계
공식문서에서는 `cmd/` 밑에 명령어 관련 파일들을 추가한다.
```js
  ▾ appName/
    ▾ cmd/
        add.go
        your.go
        commands.go // commands 는 root.go 도 가능
        here.go
      main.go
```

하지만 github cli 는 [go standars project layout](https://github.com/golang-standards/project-layout) 를 사용하고 있다.

### rootCmd
rootCmd는 이름과 같이 모든 명령어들이 합져지는 최상위 값이다.

```go
// cmd/root.go
package main

var (
  rootCmd = &cobra.Command{
		Use:     "webkeep <command> [flags]",
		Version: "0.0.1",
		Short:   "Webkeep is a archive tool for save a lot of websites to pdf light and fast",
		Long: heredoc.Doc(
			`A archive tool for save a lot of websites to pdf light and fast
			More info at https://github.com/cjaewon/webkeep`,
		),
	}
)

func init() {
  // 공식문서에서는 init 을 통해 Flags 또는 Command 추가 ... 을 하도록 소개하고 있다.
}

// main 에서 Execute 함수를 실행한다.
func Execute() {
  if err := rootCmd.Execute(); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

### Command 합치기
여러 명령어(cobra.Command)는 rootCmd 에 합칠 수 있다.
```go
// root.go
var (
  rootCmd = ...
  versionCmd = ...
)

func init() {
  rootCmd.AddCommand(versionCmd)
}
```

### Flags
Flag는 `-m / --message` 같이 명령어 뒤에 오는 수식어이다.

```go
// roog.go

var (
  rootCmd = ...
  username string 
  
  // struct 를 사용하는 경우도 있다.
  rootCmdFlags = struct {
		configFile string
	}{} 
)

func init() {
  root.Flags().StringVarP(&username, "username", "u", "기본값", "Username for using at github")
}
```

