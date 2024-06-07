## Однонаправленная ассоциация "один ко многим"

Если вы используете `@OneToMany` аннотацию с `@JoinColumn`, то у вас будет однонаправленная связь, подобная связи между родительским `Post` объектом и дочерним `PostComment` объектом на следующей диаграмме:

[![Однонаправленная ассоциация "один ко многим"](https://i.sstatic.net/ljaHg.png)](https://i.sstatic.net/ljaHg.png)

При использовании однонаправленной ассоциации "один ко многим" сопоставляет ассоциацию только родительская сторона.

В этом примере только `Post` объект будет определять `@OneToMany` ассоциацию с дочерним `PostComment` объектом:

```java
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
@JoinColumn(name = "post_id")
private List<PostComment> comments = new ArrayList<>();
```

## Двунаправленная ассоциация "один ко многим"

Если вы используете `@OneToMany` с `mappedBy` набором атрибутов, у вас получается двунаправленная связь. В нашем случае как у `Post` объекта есть коллекция `PostComment` дочерних объектов, так и у дочернего `PostComment` объекта есть обратная ссылка на родительский `Post` объект, как показано на следующей диаграмме:

[![Двунаправленная ассоциация "один ко многим"](https://i.sstatic.net/i5YWM.png)](https://i.sstatic.net/i5YWM.png)

В `PostComment` сущности свойство `post` entity отображается следующим образом:

```java
@ManyToOne(fetch = FetchType.LAZY)
private Post post;
```

> Причина, по которой мы явно устанавливаем для `fetch` атрибута значение `FetchType.LAZY`, заключается в том, что по умолчанию все `@ManyToOne` и `@OneToOne` ассоциации извлекаются быстро, что может вызвать N + 1 проблем с запросами.

В `Post` сущности `comments` ассоциация отображается следующим образом:

```java
@OneToMany(
    mappedBy = "post",
    cascade = CascadeType.ALL,
    orphanRemoval = true
)
private List<PostComment> comments = new ArrayList<>();
```

`mappedBy` Атрибут `@OneToMany` аннотации ссылается на `post` свойство дочерней `PostComment` сущности, и, таким образом, Hibernate знает, что двунаправленная связь контролируется `@ManyToOne` стороной, которая отвечает за управление значением столбца внешнего ключа, на котором основана эта табличная связь.

Для двунаправленной ассоциации вам также необходимо иметь два служебных метода, таких как `addChild` и `removeChild`:

```java
public void addComment(PostComment comment) {
    comments.add(comment);
    comment.setPost(this);
}

public void removeComment(PostComment comment) {
    comments.remove(comment);
    comment.setPost(null);
}
```

Эти два метода гарантируют, что обе стороны двунаправленной ассоциации синхронизированы. Без синхронизации обоих концов Hibernate не гарантирует, что изменения состояния ассоциации будут распространяться в базу данных.

## Какой из них выбрать?

Однонаправленная `@OneToMany` ассоциация работает не очень хорошо, поэтому вам следует избегать ее.

Вам лучше использовать двунаправленный `@OneToMany` который более эффективен.