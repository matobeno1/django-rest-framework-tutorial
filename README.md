# Django REST framework tutorial

https://www.django-rest-framework.org/tutorial/1-serialization/

# Useful

Curl alternative

```bash
pip install httpie
http http://127.0.0.1:8000/snippets/
```

# Progress

1. Initial setup
2. Created serializers.py - way to serialize/deserialize instances.
    1. `serializers.Serializer` - manually defining properties, later we can use `ModelSerializer`
    2. AFter we created model instance - we can serialize into bytes.
   ```python3
   serializer = SnippetSerializer(instance)
   content = JSONRenderer().render(serializer.data)
   ```
    3. And also deserialize it
   ```python
   stream = io.BytesIO(content)
   data = JSONParser().parse(stream)
   serializer = SnippetSerializer(data=data)
   serializer.is_valid()
   serializer.validated_data # ordered dict
   serializer.save() # saves instance in the database
   ```
    4. This works with multiple instances too: `serializer = SnippetSerializer(Snippet.objects.all(), many=True)`
3. Moving to **ModelSerializer** - just like Form has ModelForm, Serializer has ModelSerializer = code is more concise
    1. We can print the actual code of model serializer: `print(repr(SnippetSerializer()))`
4. Writing regular Django views using our Serializer


1. **Part 2** - Introduction to `Request` and `Response` classes. `Response` renders content based on requested
   content-type.
    1. Use constants for http status codes rather than numeric values!
2. Rewrite views to use mixins and Request response classes.
3. Add `format_suffix_patterns`
    1. this helps with specifying whether we want response in json or browsable api format (html)


1. **Part 3**
    1. Mixins replace most of the basic verbose functionality. See each `views.py` file

## Part 4

- Code snippets are always associated with a creator.
- Only authenticated users may create snippets.
- Only the creator of a snippet may update or delete it.
- Unauthenticated requests should have full read-only access.

### Associating Snippets with Users - override in SnippetListView:

```python
 def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
```

### Also nice to notice - adding list of ids from snippets that user has created. These are the FK's in snippets table,

representing ID of user.

```python
# User serializer
snippets = serializers.PrimaryKeyRelatedField(many=True, queryset=Snippet.objects.all())
```

### Adding readonly key for snippet GET requests - the owner field

This field will be only used when GETting data, it will be omitted on PUT.

```python
owner = serializers.ReadOnlyField(source='owner.username')
```
Alternative: `CharField(read_only=True)`

### Permissions
- `permission_classes = [permissions.IsAuthenticatedOrReadOnly]` - will ensure that authenticated requests 
get read-write access, and unauthenticated requests get read-only access (still will display data).

### Adding login to the Browsable API
```python
urlpatterns += [
    path('api-auth/', include('rest_framework.urls')),
]
```


