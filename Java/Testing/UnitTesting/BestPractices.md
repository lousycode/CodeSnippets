# Mocking Static Builders
```
public class AnswerWithSelf implements Answer<Object> {
        private final Answer<Object> delegate = new ReturnsEmptyValues();
        private final Class<?> clazz;

        public AnswerWithSelf(Class<?> clazz) {
            this.clazz = clazz;
        }

        public Object answer(InvocationOnMock invocation) throws Throwable {
            Class<?> returnType = invocation.getMethod().getReturnType();
            if (returnType == clazz) {
                return invocation.getMock();
            } else {
                return delegate.answer(invocation);
            }
        }
    }

```

Static builders can be used when we need to mock a static builder like `Class.Builder.build()`. 

```
Class.Builder builder = mock(Class.Builder.class, new AnswerWithSelf(Class.Builder.class));
```

