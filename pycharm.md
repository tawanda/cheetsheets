To profile a rest endpoint method you could look into my repos utilities/profile.py and use the decorator there


```
class MyView(APIView):

    @profileit
    def get(self, request, **kwargs):
      ...
```      
