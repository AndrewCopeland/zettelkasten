# 1584724813 bash-cheat-sheet
#bash #cheat-sheet

Get all files names within a directory
```bash
find $ZETTEL_DIR  -type f -exec basename {} \;
```

Searching for mutliple values using grep
```bash
grep -e "field1|field2"
```

Creating a multilined string:
```bash
multi_lined_string=$(cat <<-END
line1
line2
## Links
END
)
```


## Links
