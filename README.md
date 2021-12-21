# Django REST framework tutorial

https://www.django-rest-framework.org/tutorial/1-serialization/

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
4. b
5. c
6. d