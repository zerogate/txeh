# Etc Hosts Management Library

This small go library was developed to encapsulate the complexity of
working with /etc/hosts by providing a simple interfact to load, add or remove rntries and save a /etc/host file. No validation is done on the input data and is considered out of scope for this project.

Basic implemention:
```go

package main

import (
	"fmt"
	"strings"

	"github.com/txn2/txeh"
)

func main() {
	hosts := txeh.NewHostsDefault()

	hosts.AddHost("127.100.100.100", "test")
	hosts.AddHost("127.100.100.101", "logstash")
	hosts.AddHosts("127.100.100.102", []string{"a", "b", "c"})
	
	hosts.RemoveHosts([]string{"example", "example.machine", "example.machine.example.com"})
	hosts.RemoveHosts(strings.Fields("example2 example.machine2 example.machine.example.com2"))

	
	hosts.RemoveAddress("127.1.27.1")
	
	removeList := []string{
		"127.1.27.15",
		"127.1.27.14",
		"127.1.27.13",
	}
	
	hosts.RemoveAddresses(removeList)
	
	hfData := hosts.RenderHostsFile()

	fmt.Println(hfData)
	
	hosts.SaveAs("./test.hosts")
}

```