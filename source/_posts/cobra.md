---
title: 使用Cobra构建命令行工具
date: 2021-01-03 19:37:25
tags: [golang, cobra]
---

[Cobra][]是一个Golang包，它提供了简单的接口来创建命令行程序。
[Cobra][]还是一个应用程序，用来生成应用框架，从而开发以[Cobra][]为基础的应用。

[Cobra]: https://github.com/spf13/cobra

## 安装

使用下面命令安装`Cobra`：

```bash
go get -u github.com/spf13/cobra
```

## 快速初始化

执行下面命令快速初始化命令：

```bash
cobra init myCmd --pkg-name myCmd
```

<!--more-->

目录结构如下：

```txt
myCmd/
├── LICENSE
├── cmd/
│   └── root.go
└── main.go
```

其中`root.go`文件内容如下：

```go
package cmd

import (
	"fmt"
	"github.com/spf13/cobra"
	"os"

	homedir "github.com/mitchellh/go-homedir"
	"github.com/spf13/viper"
)

var cfgFile string

// rootCmd represents the base command when called without any subcommands
var rootCmd = &cobra.Command{
	Use:   "myCmd",
	Short: "A brief description of your application",
	Long: `A longer description that spans multiple lines and likely contains
examples and usage of using your application. For example:

Cobra is a CLI library for Go that empowers applications.
This application is a tool to generate the needed files
to quickly create a Cobra application.`,
	// Uncomment the following line if your bare application
	// has an action associated with it:
	//	Run: func(cmd *cobra.Command, args []string) { },
}

// Execute adds all child commands to the root command and sets flags appropriately.
// This is called by main.main(). It only needs to happen once to the rootCmd.
func Execute() {
	if err := rootCmd.Execute(); err != nil {
		fmt.Println(err)
		os.Exit(1)
	}
}

func init() {
	cobra.OnInitialize(initConfig)

	// Here you will define your flags and configuration settings.
	// Cobra supports persistent flags, which, if defined here,
	// will be global for your application.

	rootCmd.PersistentFlags().StringVar(&cfgFile, "config", "", "config file (default is $HOME/.myCmd.yaml)")

	// Cobra also supports local flags, which will only run
	// when this action is called directly.
	rootCmd.Flags().BoolP("toggle", "t", false, "Help message for toggle")
}

// initConfig reads in config file and ENV variables if set.
func initConfig() {
	if cfgFile != "" {
		// Use config file from the flag.
		viper.SetConfigFile(cfgFile)
	} else {
		// Find home directory.
		home, err := homedir.Dir()
		if err != nil {
			fmt.Println(err)
			os.Exit(1)
		}

		// Search config in home directory with name ".myCmd" (without extension).
		viper.AddConfigPath(home)
		viper.SetConfigName(".myCmd")
	}

	viper.AutomaticEnv() // read in environment variables that match

	// If a config file is found, read it in.
	if err := viper.ReadInConfig(); err == nil {
		fmt.Println("Using config file:", viper.ConfigFileUsed())
	}
}
```

## 添加子命令

使用下面命令添加子命令：

```bash
cobra add subCmd
```

目录结构如下：

```txt
myCmd/
├── LICENSE
├── cmd
│   ├── root.go
│   └── subCmd.go
└── main.go
```

`subCmd.go`文件内容如下：

```go
package cmd

import (
	"fmt"

	"github.com/spf13/cobra"
)

// subCmdCmd represents the subCmd command
var subCmdCmd = &cobra.Command{
	Use:   "subCmd",
	Short: "A brief description of your command",
	Long: `A longer description that spans multiple lines and likely contains examples
and usage of using your command. For example:

Cobra is a CLI library for Go that empowers applications.
This application is a tool to generate the needed files
to quickly create a Cobra application.`,
	Run: func(cmd *cobra.Command, args []string) {
		fmt.Println("subCmd called")
	},
}

func init() {
	rootCmd.AddCommand(subCmdCmd)

	// Here you will define your flags and configuration settings.

	// Cobra supports Persistent Flags which will work for this command
	// and all subcommands, e.g.:
	// subCmdCmd.PersistentFlags().String("foo", "", "A help for foo")

	// Cobra supports local flags which will only run when this command
	// is called directly, e.g.:
	// subCmdCmd.Flags().BoolP("toggle", "t", false, "Help message for toggle")
}
```
