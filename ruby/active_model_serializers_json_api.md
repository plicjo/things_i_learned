# Active Model Serializers

## JSON API Adapter

When using the JSON API adapter, [you won't get any nested resources](https://github.com/rails-api/active_model_serializers/issues/1948). This is a feature, not a bug. You don't nest resources in the JSON API spec.


### Serializers
```ruby
class AuthorSerializer < ActiveModel::Serializer
  has_many ::books
end

class BookSerializer < ActiveModel::Serializer
  attribute(:publisher_name) { object.publisher.name }
end
```

*API Response*
```json
"relationships": {
  "books": {
    "data": [
      {
        "id": "821",
        "type": "books"
      },
```

The attributes won't get returned! [#JSONAPISpec](http://jsonapi.org/)

### Nested Resources

There's a bug when showing nested resources with the JSON API adapter in Active Model Serializers.
It does a deeply nested n+1 query [by default](https://github.com/rails-api/active_model_serializers/issues/1325).

To get away from the n+1, you can specify an `include_data false` inside of the relationship definition.

*Avoiding the n+1 query bug*

```ruby
class PostSerializer < ActiveModel::Serializer
  attributes :title, :body
  has_many :comments do
    link(:related)  { post_comments_path(post_id: object.id) }
    include_data false
  end
end
```
