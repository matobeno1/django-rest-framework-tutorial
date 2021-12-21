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

**Part 4**

- Code snippets are always associated with a creator.
- Only authenticated users may create snippets.
- Only the creator of a snippet may update or delete it.
- Unauthenticated requests should have full read-only access.

