# Hello world

## Output

```
PS D:\Learn\go\examples\hello> .\hello.exe test
Hello test
456
123
```

## Program

```go
package main
import (
	"fmt"
	"os"
	"strings"
)

func main() {
	who := "World!"
	if len(os.Args) > 1 {
		who = strings.Join(os.Args[1:], " ")
	}
	fmt.Println("Hello", who)

	test := "123"
	if true {
		test := "456"
		fmt.Println(test)
	}
	fmt.Println(test)
}
```
