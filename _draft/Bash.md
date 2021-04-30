# Bash

## Variables

#### global variables

```
variable=value
```

#### local variables

```
local variable=value
```



## Flow control

### if, case

#### if

```
if commands; then
	commands
[elif commands; then
	commands]
[else
	commands]
fi
```

#### case

```
case word in
	[pattern [| pattern]...) commands ;;]...
esac
```



### while/until, for

#### while

true enter loop.

```
while commands; do
	commands
done

Or

while (( expression )); do
	commands
done
```

#### until

true leave loop.

```
until commands; do
	commands
done
```

#### for

```3
for variable [in words]; do
	commands
done

Or 

for ((expression1; expression2; expression)); do
	commands
done
```



## Fuction

```
name () {
	commands
	return
}
```

