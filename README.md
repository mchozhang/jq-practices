# jq-practices
sample jq practices

## Questions

1. Pretty-print/format

```shell
 jq . list-functions.json
```

2. Print the list of FunctionName
```shell
 jq '[.Functions[] | .FunctionName]' list-functions.json
```

3. Count the number of functions
```shell
 jq '.Functions | length' list-functions.json 
```

4. For every Lambda function that has a Runtime of (exactly)python3.8, print out its FunctionName
```shell
jq '.Functions[] | select(.Runtime == "python3.8") | .FunctionName' list-functions.json
jq '.Functions[] | select(.Runtime == "python3.8").FunctionName' list-functions.json 
```

5. For every Lambda function that has a Runtime that starts with python (even future versions, don't hardcode a list), 
   print out its FunctionName
   
```shell
jq '.Functions[] | select(.Runtime | contains("python")) | .FunctionName' list-functions.json
```

6. Assuming you have an environment variable called `RUNTIME` that contains `python3.8`, 
   solve Q4 by using the jq parameter --arg (you are not allowed to cheat and use `bash` parameter expansion within the `jq` filter`.
   
```shell
RUNTIME=python3.8 && jq --arg r "$RUNTIME" '.Functions[] | select(.Runtime == $r) | .FunctionName' list-functions.json
```

7. Solve Q5, but rather than operate on a single file, operate on every file as a single stream. 
   Hint, use the `jq` parameter `--slurp`.  You want something like: `cat list-functions.json | jq -rs 'FILTER'`
   
```shell
jq -s '.[] | .Functions[] | select(.Runtime | contains("python")) | .FunctionName' list-functions*.json
```

## todo
* options: `-r`, `-n`, `-e`
* filters: `//`, `empty`, `map`, `@csv`, `@tsv`, `to_entries`, `from_entries`, `sort_by`, `fromdateiso8601`, `match`