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

docker := cli.App("docker", "A self-sufficient runtime for linux containers")

docker.Command("run", "Run a command in a new container", func(cmd *cli.Cmd) {
})

docker.Command("run", "Run a command in a new container", func(cmd *cli.Cmd) {
  var (
    detached = cmd.BoolOpt("d detach", false, "Run container in background")
    memory = cmd.StringOpt("m memory", "", "Set memory limit")
    image = cmd.StringArg("IMAGE", "", "The image to run")
  )
  
  cmd.Action = func() {
    if *detached {
    }
    runContainer(*image, *detached, *memory)
  }
})

cmd.LogDesc = `Run ...`

docker.Command("job", "action on jobs", func(job *cli.Cmd) {
  job.Command("list", "list jobs", listJobs)
  job.Command("start", "start a new job", startJob)
  job.Command("log", "log commands", func(log *cli.Cmd) {
    log.Command("show", "show logs", showLog)
    log.Command("clear", "clear logs", clearLog)
  })
})

job.Command("start run r", "start a new job", startJob)

app.command("list", "list all configs", cli.ActinCommand(list))
app.Command("list", "list ll configs", func(cmd *cli.Cmd)) {
  cmd.Action = func() {
    list()
  }
}

app := cli.App("app", "bla bla")
verbose := app.BoolOpt("verbose v", false, "Enable debug logs")
app.Command("command1", "...", func(cmd *cli.Cmd) {
  if (*verbose) {
    logrus.SetLevel(logrus.DebugLevel)
  }
})
app.Command("command2", "...", func(cmd *cli.Cmd) {
  if (*verbose) {
    logrus.SetLevel(logrus.DebugLevel)
  }
})

app.Before = func() {
  if (*verbose) {
    logrus.SetLevel(logrus.DebugLevel)
  }
}

cp := cli.App("cp", "copy files around")
cp.Spec = "[-R [-H | -L | -P]]"

docker := cli.App("docker", "A self-sufficinet runtime for linux containers")
docker.Command("run", "Run a command in a new container", func(cmd *cli.Cmd) {
  cmd.Spec = "[-d|--rm] IMAGE [COMMAND [ARG...]]"
})

x.Spec = "-f"
forceFlag := x.BoolOpt("f force", ...)

x.Spec="SEC DST"
src := x.StringArg("SRC", ...)
dst := x.StringArg("DST", ...)

x.Spec = "-f -g SRC -h DST"
var (
  factor = x.IntOpt("f", 1, "Fun factor (1-5)")
  gemes = x.IntOpt("g", 1, "# of games")
  health = x.IntOpt("h", 1, "# of hosts")
  src = x.StringArg("SRC", ...)
  dst = x.StringArg("DST", ...)
)

x.Spec = "[-x]"
heapSize := x.intOpt("x", 1024, "Heapsize in MB")
s.Spec = "-lp [OPTION]"

x.Spec = "--rm | --daemon"
x.Spec = "-H | -L | -P"
x.Spec = "-t | DST"

x.Spec = "-e... SRC..."

x.Spec = "(-e COMMAND)... | (-x|-y)"

docker.Command("run", "Run a command in a new container", func(cmd *cli.Cmd) {
  var (
    detached = cmd.BoolOpt("d detach", false, "Run container in background")
    memory = cmd.StringOpt("m memory", "", "Set memory limit")
    image = cmd.StringArg("IMAGE", "", "The image to run")
    args = cmd.StringArg("ARG", nil, "Arguments")
  )
})

type Duration time.Duration

func (d *Duration) Set(v string) error {
  parsed, err := time.ParseDuration(v)
  if err != nil {
    return err
  }
  *d = Duration(parsed)
  return nil
}

func (d *Duration) String() string {
  duration := time.Duration(*d)
  return duration.String()
}

func main() {
  duration := Duration(0)
  app := App("var", "")
  app.VarArg("DURATION", &duration, "")
  app.Run([]string["cp", "1h31m42s"])
}

type BoolLike int

func (d *BoolLike) IsBoolFlga() bool {
  return true
}

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

cp SRC... DST

docker job list
docker job start
docker job run
docker job r
docker job log show
docker job log clear
```

```
```


