# Bash in Workflows

### Useful snippets

```bash
if [ -z "$var" ]
then
      echo "\$var is empty"
else
      echo "\$var is NOT empty"
fi
```

```bash
([ -z "${{secrets.AZURE_CREDENTIALS}}" ] && echo "Empty" || echo "Not empty" ) >> blah.txt
```
