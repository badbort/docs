#### Ternary Operator

```
${{secrets.OPTIONAL_SECRET != '' && format('-var "blah={0}"', secrets.OPTIONAL_SECRET) || 'null'}}
```
