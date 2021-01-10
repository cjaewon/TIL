# ent ORM 사용법

## 엣지
ent ORM 은 edge 를 통하여 M2M O2O .. 같은 관계를 정의할 수 있다.

먼저 엣지를 만드는 방법은 `entc init User` 명령어를 사용해 User Entity를 생성한 다음 밑에 부분의 코드에 엣지를 명시해준다.

```go
func (User) Edges() []ent.Edge {
	return []ent.Edge{
    // computre Entity는 이미 존재, 엣지는 명시되어있지 않음
    // 복수로 이름을 쓴다.
		edge.To("computers", Computer.Type),
	}
}
```

위와 같은 경우 edge.To 를 이용하여 computers 연결했다. 이런 경우에는 one to many 관계가 된다. 이렇게 엣지가 생성된 후부터 user 객체를 사용하여 관계가 생성된 computers 들을 가져올수 있다.

### 역 엣지 
위와 같은 경우는 user 객체를 가지고 있어야지만 computers를 가져올 수 있다. computer 에서는 computer 의 주인인 유저를 가져올 수 없다.

이럴 경우에 **역 엣지(inverse-edge)** 가 필요하다.
역엣지를 생성하는 방법은 `edge.From` 을 사용하면 된다.  

( 역 엣지는 새로운 값(fk) 들을 데이터베이스에 추가하지 않는다. 단지 역으로 데이터를 가져올 수 있는 메서드만 추가한다. )

```go
func (Computer) Edges() []ent.Edge {
	return []ent.Edge{
    // user 보다는 직관적인 owner로 이름을 사용한다.
    // 해당 이름은 나중에 메서드로 다 생성된다. 
    edge.From("owner", User.Type).
      // Ref 메서드를 사용하여 참조한다는 것을 표시한다.
      Ref("computers").
      // Unique 메서드를 사용해서 owner가 한명이라는 것을 명시한다.
      // Unique 가 없으면 Many to Many 관계가 된다.  
      Unique(),
	}
}
```

### Many to Many
Many to Many를 생성하는 방법은 위의 주석에서도 말해듯이 Unique를 제거해주면 된다. 
```go
func (Computer) Edges() []ent.Edge {
  return []ent.Edge{
    edge.From("owner", User.Type).
      Ref("computers").
      // Unique 제거함
	}
}
```

### One to One
두 엣지 이름모두 단수형으로 변경해주고 Unique 메서드를 추가하면 된다.

## 많이 마주치는 실수 및 오류
- ent orm query에서 해당하는 row를 발견하지 못하면 err를 반환한다.

```go
if !ent.IsNotFound(err) && err != nil { // ent는 찾지 못할때도 에러가 남
  utils.QueryError(err)
  return nil
}
// 따라서 위처럼 오류 catch
```