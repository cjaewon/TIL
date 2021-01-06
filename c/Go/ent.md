# ent ORM 사용법

## 많이 마주치는 실수 및 오류
- ent orm query에서 해당하는 row를 발견하지 못하면 err를 반환한다.

```go
if !ent.IsNotFound(err) && err != nil { // ent는 찾지 못할때도 에러가 남
  utils.QueryError(err)
  return nil
}
// 따라서 위처럼 오류 catch
```