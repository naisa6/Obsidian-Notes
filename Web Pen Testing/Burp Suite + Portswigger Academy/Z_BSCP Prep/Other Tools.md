To find what type of file (in Linux)
```
echo -n <STRING> | base64 -d > astring
file astring
```
This may show if it is a gzip file, if it is:
```
gzip -d astring
```
Then `file astring` again and may reveal it is a Java serialization data