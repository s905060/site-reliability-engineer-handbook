# How-To rename the extension for a batch of files?

```
for file in *.html; do
    mv "$file" "`basename $file .html`.txt"
done
```