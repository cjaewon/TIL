# Typeorm 팁

## relation (관계)
Typeorm에서의 관계정의는 `@OneToOne`, `@OneToMany` 와 같은 데코레이터로 명시해 줄 수 있다. 이러한 관계를 명시해 줄 때는 다양한 옵션 값을 줄 수 있다.

- `cascade` entity에서 `@OneToOne` 같이 관계가 정의되어있는 필드값들을 수정하거나 추가하면 자동으로 연결되어있는 필드값들을 업데이트 해준다.
```ts
@Entity('users')
export default class User {
  @OneToOne(type => UserProfile, profile => profile.user, { cascade: true })
  profile!: UserProfile;
}
// cascade가 true일 경우는 다음과 같이 함께 업데이트가 가능하다.

const user: User = getUser();
user.profile.display_name = 'Cjaewon';

await getRepository(User).save(user);
// profile.display_name 도 같이 수정됨
```
- `onDelete` 외래키가 있는 경우에는 삭제할려면 관계가 있는 필드도 함께 삭제해야한다. 이를 자동화 하기 위해 `onDelete: 'CASCADE'` 로 설정하면 된다.
( `JoinColumn`이 있는 필드에 `onDelete` 를 추가해준다. )