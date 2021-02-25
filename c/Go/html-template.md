## Go언어 html/template

Go언어의 `html/template` 은 `text/template` 기반으로 XSS 이스케프 기능을 추가한 모듈이다.

### 템플릿 문법
`{{ ... }}` 같이 대괄호로 감싸져있다.

#### `{{ template "템플릿 이름" }}`
`template.Template` 에 포함된 명시 된 템플릿이면 해당 템플릿을 사용 할 수 있다.
만약 이것을 제외하고 사용하고 싶다면 다음과 같이 사용해야 한다.

```go
tmpl := t.templates.Lookup(name)

if tmpl == nil {
  err := errors.New("failed to look up html/template")
  return err
}

return t.templates.ExecuteTemplate(w, "base.html", data)
```

만약 Lookup 함수 안에 name이 base.html, ExecuteTemplate 안의 base.html이 name이 된다면 정상적으로 실행되지 않는데 그 이유는 base.html에 있는 `{{ block "main" . }}{{ end }}` 는 `{{ define "main" }}{{ end }}` 있어야지만 실행되기 때문이다.