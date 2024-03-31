---
layout:   page
title:       "使用viper来管理你的配置"
subtitle:    ""
description: ""
order: 1
date: 2024-3-31
author:   "aiqiu"
#image:       "https://golearning.oss-cn-shanghai.aliyuncs.com/202304161638319.png"
categories:  ["Golang"]
published: true
---

# 使用viper来管理你的配置
## 什么是viper
Viper 是适用于 Go 应用程序（包括 12 因素应用程序）的完整配置解决方案。它设计用于在应用程序中工作，可以处理所有类型的配置需求和格式。它支持
* 设置配置默认值
* 读取 JSON、TOML、YAML、HCL、envfile 和 Java 属性配置文件
* 实时监测和重新读取配置文件
* 从远程配置系统（etcd 或 Consul）读取，并监测修改
* 从命令行标志读取环境变量

## 如何使用viper
1. 首先引入依赖
```shell
go get -u github.com/spf13/viper
```

2.创建一个配置文件取名（这里的名字可以自己定义）`confi.yaml`， 这个名字将会被用代码初始化之中。 在代码中，需要构建出配置对应结构体。如下
```yaml
OPENAI:
OPENAI_API_KEY: ""


bilibili:
BILIBILI_COOKIE: ""
```
```go
type Config struct {
OPENAIConfig   OPENAIConfig   `mapstructure:"openai"`
BilibiliConfig BilibiliConfig `mapstructure:"bilibili"`
}

type OPENAIConfig struct {
ApiKey string `mapstructure:"openai_api_key"`
}

type BilibiliConfig struct {
SessionToken string `mapstructure:"BILIBILI_COOKIE"`
}
```

这边注意的是：tag需要使用`mapstructure` 而不能使用`yaml`，因为viper内部使用的是`mapstructure`来解析的数据

3.构建读取逻辑
```go
var GlobalConfig Config

func init() {
// load config from file
viper.SetConfigName(configFile)
// 从配置的目录中读取
viper.AddConfigPath(configFolder)
viper.SetConfigType("yaml")

	if err := viper.ReadInConfig(); err != nil {
		logger.Panicf("Failed to read config file: %s", err)
	}

	// unmarshal config
	if err := viper.Unmarshal(&GlobalConfig); err != nil {
		logger.Panicf("Failed to unmarshal config: %s", err)
	}
	logger.Infof("Config loaded: %+v", GlobalConfig)
}
```
