# Active Model Serializers

## JSON API Adapter

When using the JSON API adapter, [you won't get any nested resources](https://github.com/rails-api/active_model_serializers/issues/1948). This is a feature, not a bug. You don't nest resources in the JSON API spec.


*Serializers*
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
