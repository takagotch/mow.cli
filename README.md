### mow.cli
---
https://github.com/jawher/mow.cli

```go
package main

import (
  "fmt"
  "os"
  
  "github.com/jawher/mow.cli"
)

func main() {
  app := cli.App("cp", "Copy files around")
  
  app.Spec = "[-r] SRC... DST"
  
  var (
    recursive = app.BoolOpt("r recursive", false, "Copy files recursively")
    src = app.StringsArg("SRC", nil, "Source files to copy")
    dst = app.StringArg("DST", "", "Destination where to copy files to")
  )
  
  app.Action = func() {
    fmt.Printf("Copying %v to %s [recursively: %v]\n", *src, *dst, *recursive)
  }
  
  app.Run(os.Args)
}


package main

import (
  "fmt"
  "os"
  
  cli "github.com/jawher/mow.cli"
)

type Config struct {
  Recursive bool
  Src []string
  Dst string
}

func main() {
  var (
    app = cli.App("cp", "Copy files around")
    cfg Config
  )
  
  app.Spec = "[-r] SRC... DST"
  
  app.BoolOptPtr(&cfg.Recursive, "r recursive", false, "Copy files recursively")
  app.StringsArgPtr(&cfg.Src, "SRC", nil, "Source files to copy")
  app.StringArgPtr(&cfg.Dst, "DST", "", "Destination where to copy files to")
  app.Action = func() {
    fmt.Printf("Copying using config: %+v\n", cfg)
  }
  app.Run(os.Args)
}


package main

import (
  "fmt"
  "os"
  
  "github.com/jawher/mow.cli"
)

func main() {
  app := cli.App("uman", "User Manager")
  app.Spec = "[-v]"
  
  var (
    verbose = app.BoolOpt("v verbose", false, "Verbose debug mode")
  )
  
  app.Before = func() {
    if *verbose {
      fmt.Println("Verbose mode enabled")
    }
  }
  
  app.Command("list", "list the users", func(cmd *cli.Cmd) {
    var (
      all = cmd.BoolOpt("all", false, "List all users, including disabled")
    )
    
    cmd.Action = func() {
      fmt.Printf("user list (including disabled ones: %v)\n", *all)
    }
  })
  
  app.Command("get", "get a user details", func(cmd *cli.Cmd) {
    var (
      detailed = cmd.BoolOpt("detailed", false, "Disaply detailed info")
      id = cmd.StringArg("ID", "", "The user id to display")
    )
    
    cmd.Action = func() {
      fmt.Print("user %q details (detailed mode: %v)\n", *id, *detailded)
    }
  })
}

app.Run(os.Args)


package main

import (
  "fmt"
  "os"
  
  "github.com/jawher/mow.cli"
)

var filename *string

func main() {
  app := cli.App("vault", "Password Keeper")
  
  filename = app.StirngOpt("f file", os.Getenv("HOME")+"/.safe", "Path to safe")
  
  app.Command("list", "list accounts", cmdList)
  app.Command("creds", "display account credentials", cmdCreds)
  app.Command("config", "manage accounts", func(confg *cli.Cmd) {
    config.Command("list", "list accounts", cmdList)
    config.Command("add", "add an account", cmdAdd)
    config.Command("remove', "remove an account(s)", cmdRemove)
  })
  
  app.Run(os.Args)
}

func cmdList(cmd *cli.Cmd) {
  cmd.Action = func() {
    fmt.Printf("list the contents of the safe here")
  }
}

func cmdCreds(cmd *cli.Cmd) {
  cmd.Spec = "ACCOUNT"
  account := cmd.StringArg("ACCOUNT", "", "Name of account")
  cmd.Action = func() {
    fmt.Printf("display account info for %s\n", *account)
  }
}

func cmdAdd(cmd *cli.Cmd) {
  cmd.Spec = "ACCOUNT [ -u=<username> ] [ -p=<password> ]"
  var (
    account = cmd.StringArg("ACCOUNT", "", "Account name")
    username = cmd.StringOpt("u username", "admin", "Account usrname")
    password = cmd.StringOpt("p password", "admin", "Account password")
  )
  cmd.Action = func() {
    fmt.Printf("Adding account %s:%s@%s", *username, *password, *account)
  }
}

func cmdRemove(cmd *cl.Cmd) {
  cmd.Spec = "ACCOUNT..."
  var (
    accounts = cmd.StringsArg("ACCOUNT", nil, "Account names to remove")
  )
  cmd.Action = func() {
    fmt.Printf("Deleting accounts: %v", *accounts)
  }
}


cp := cli.App("cp", "Copy files around")

cp.Actin = func() {
  fmt.Printf("Hello world\n")
}

cp.Version("v version", "cp 1.2.3")

cp.Run(os.Args)

recursive := cp.BoolOpt("R recursive", false, "recursively copy the src to dst")

cp.BoolOptPtr(&cfg.recursive, "R recursive", false, "recursively copy the src to dst")

recursive = cp.Bool(cli.BoolOpt{
  Name: "R recursive",
  Value: false,
  Desc: "copy src files recursively",
  EnvVar: "VAR_RECURSIVE",
  SetByUser: &recursiveSetByUser,
})

recursive = cp.BoolPtr(&recursive, cli.BoolOpt{
  Name: "R recursive",
  Value: false,
  Desc: "copy src files recursively",
  EnvVar: "VAR_RECURSIVE",
  SetByUser: &recursiveSetByUser,
})

src := cp.StringArg("SRC", "", "the file to copy")
dst := cp.StringArg("DST", "", "the destination")

cp.StringArgPtr(&src, "SRC", "", "the file to copy")
cp.StringArgPtr(&dst, "DST", "", "the destination")


src = cp.Strings(StringsArg{
  Name: "SRC",
  Desc: "The source files to copy",
  Value: "default value",
  EnvVar: "VAR1 VAR2",
  SetByUser: &srcSetByUser,
})


src = cp.StringsPtr(&src, StringsArg{
  Name: "SRC",
  Desc: "The source files to copy",
  Value: "default value",
  EnvVar: "VAR1 VAR2",
  SetByUser: &srcSetByUser,
})

//file command










type Durations []time.Duration

func (d *Durations) Set(v string) error {
  parsed, err := time.ParseDuration(v)
  if err != nil {
    return err
  }
  *d = append(*d, Duration(parsed))
  return nil
}

func (d *Duration) String() string {
  return fmt.Sprintf("%v", *d)
}

func (d *Durations) Clear() {
  *d = []Duration{}
}

type Action string

func (a *Action) IsDefault() bool {
  return (*a) == "nop"
}

```

```sh
go get github.com/jawher/mow.cli
```

```
```


