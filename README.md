```
<ul>
  <li.Content
    .includes(:likes, :authors)
    .select('likes.user_id == current_user.id as liked?')
    .each.content
    @="content"
  >
    <img src={content.image_url} alt={content.name}>
    <h2>{content.name}</h2>

    <.partial.like-star
      content={content}
      afterLike={content.query.liked: true}
      refresh="@content"
    />

    <a href="" onClick={content.client.more.toggle()}>more...</a>
    <div.if.content.client.more?>
      <div>
        Written by
        <.content.authors.each.author>
          <.if.content.authors.length > 2 && author.client.last?>
            and
          </.if>
          {author.name},
        </.content.authors>
      </div>
    </div>
    <button onClick={content.destroy()} refresh="@content">Delete</button>
  </li.Content.all>
</ul>
```

```
<ul>
  <li id="content-1">
    <img src="https://example.com/test.jpg" alt="A Cat">
    <h2>A Cat</h2>

    <img hidden src="/star.png" alt="Star" />

    <img
      src="/empty-star.png"
      onclick="API.post('/likes', { like: { content_id: 1 }, _client: { more: true }, _success: { query.liked: true }, refresh: '@content' })"
    />

    <a href="" onclick="helper.toggle('content-1')">more...</a>

    <div hidden>
      <div>
        Written by bookis, shane, and you
      </div>
    </div>
    <form action="/contents/1" method="DELETE">
      <button>Delete</button>
    </form>
  </li.Content.all>
</ul>
```

```
POST /likes
{
  like: { content_id: 1 }
  _refresh: '@content',
  _client: {
    more: true,
  }
  _success: {
    query.liked: true,
  },
}
```

```
class Content
  def liked?(user)
    likes.find { |like| like.user_id == user.id }
  end
end
```
