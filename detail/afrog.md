## afrog <https://github.com/zan8in/afrog>
<!--auto_detail_badge_begin_0b490ffb61b26b45de3ea5d7dd8a582e-->
![Language](https://img.shields.io/badge/Language-Golang-blue)
![Author](https://img.shields.io/badge/Author-zan8in-orange)
![GitHub stars](https://img.shields.io/github/stars/zan8in/afrog.svg?style=flat&logo=github)
![Version](https://img.shields.io/badge/Version-V2.9.5-red)
![Time](https://img.shields.io/badge/Join-20220615-green)
<!--auto_detail_badge_end_fef74f2d7ea73fcc43ff78e05b1e7451-->

## What is afrog

afrog is a high-performance vulnerability scanner that is fast and stable. It supports user-defined PoC and comes with several built-in types, such as CVE, CNVD, default passwords, information disclosure, fingerprint identification, unauthorized access, arbitrary file reading, and command execution. With afrog, network security professionals can quickly validate and remediate vulnerabilities, which helps to enhance their security defense capabilities.

## Features

* [x] Open source
* [x] Fast, stable, with low false positives
* [x] Detailed HTML vulnerability reports
* [x] Customizable and stably updatable PoCs
* [x] Active community exchange group

## Installation

### Prerequisites

- [Go](https://go.dev/) version 1.19 or higher.

you can install it with:

**Binary**
```sh
$ https://github.com/zan8in/afrog/releases
```

**Github**
```sh
$ git clone https://github.com/zan8in/afrog.git
$ cd afrog
$ go build cmd/afrog/main.go
$ ./afrog -h
```

**Go**
```sh
$ go install -v https://github.com/zan8in/afrog/cmd/afrog@latest
```

## Running afrog

By default, afrog scans all built-in PoCs, and if it finds any vulnerabilities, it automatically creates an HTML report with the date of the scan as the filename.

```sh
afrog -t https://example.com
```

**Warning occurs when running afrog**

If you see an error message saying:
```
[ERR] ceye reverse service not set: /home/afrog/.config/afrog/afrog-config.yaml
```
it means you need to modify the [configuration file](#configuration-file).

To execute a custom PoC directory, you can use the following command:

```sh
afrog -t https://example.com -P mypocs/
```

Use the command `-s keyword` to perform a fuzzy search on all PoCs and scan the search results. Multiple keywords can be used, separated by commas. For example: `-s weblogic,jboss`.

```sh
afrog -t https://example.com -s weblogic,jboss
```

Use the command `-S keyword` to scan vulnerabilities based on their severity level. Severity levels include: `info`, `low`, `medium`, `high`, and `critical`. For example, to only scan high and critical vulnerabilities, use the command `-S high,critical`.

```sh
afrog -t https://example.com -S high,critical
```

You can scan multiple URLs at the same time as well.

```sh
afrog -T urls.txt
```

## Configuration file

The first time you start afrog, it will automatically create a configuration file called `afrog-config.yaml`, which will be saved in the current user directory under `$HOME/.config/afrog/afrog-config.yaml`.

Here is an example config file:

```yaml
reverse:
  ceye:
    api-key: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    domain: "xxxxxx.ceye.io"
```

`reverse` is a reverse connection platform used to verify command execution vulnerabilities that cannot be echoed back. Currently, only ceye can be used for verification. To obtain ceye, follow these steps:

- Go to the [ceye.io](http://ceye.io/) website and register an account.
- Log in and go to the personal settings page.
- Copy the `domain` and `api-key` and correctly configure them in the `afrog-config.yaml` file.


## Json Output (For developers)

### Json
Optional command: `-json` `-j`, Save the scan results to a JSON file. The JSON file includes the following contents by default: `target`, `fulltarget`, `id`, and `info`. The info field includes the following sub-fields: `name`, `author`, `severity`, `description`, and `reference`. If you want to save both `request` and `response` contents, please use the [-json-all](#jsonall) command parameter.

```sh
afrog  -t https://example.com -json result.json
afrog  -t https://example.com -j result.json
```

::: warning
The content of the JSON file is updated in real time. However, there is an important note to keep in mind: before the scan is completed, if developers want to parse the file content, they need to add a '`]`' symbol to the end of the file by themselves, otherwise it will cause parsing errors. Of course, if you wait for the scan to complete before parsing the file, this issue will not occur.
:::


### JsonAll

Optional command: `-json-all` `-ja`, The only difference between the `-json-all` and `-json` commands is that `-json-all` writes all vulnerability results, including `request` and `response`, to a JSON file.

```sh
afrog -t https://example.com -json-all result.json
afrog -t https://example.com -ja result.json
```


## Screenshot

![](https://github.com/zan8in/afrog/blob/main/images/1.png)

<!--auto_detail_active_begin_e1c6fb434b6f0baf6912c7a1934f772b-->
## 项目相关

- 2023-07-10 发布视频[《404星链计划-afrog：快速稳定的漏洞扫描工具》](https://www.bilibili.com/video/BV1Pz4y177PU/)

## 最近更新

#### [v2.9.5] - 2023-12-06

**更新**  
- 新增 -cyberspace/-cs 网络测绘空间搜索功能  
- 优化 PoC GitLab public snippets 漏洞等级由 INFO 改为 HIGH

#### [v2.9.3] - 2023-11-27

**更新**  
- 新增 -sort 参数，命令 -sort a-z 按 PoC 首字母顺序扫描，默认按 PoC 漏洞等级从低到高顺序扫描  
- 新增 versionCompare 函数，用于比较版本号大小  
- 新增 ActiveMQ RCE 漏洞检测  
- 优化 -web 命令的报告模板，使其与 report 模板一致

#### [v2.9.2] - 2023-11-16

**更新**  
- 新增 debug 参数，它可以在执行过程中打印更详细的请求和响应信息  
- 优化响应 Header 头包含多个 Set-Cookie 情况，合并到一个 Set-Cookie

#### [v2.9.1] - 2023-11-01

**更新**  
- 为解决 2.9.0 版本干扰漏洞探测结果的问题，建议升级至 2.9.1，或使用更低版本  
- 新增 -resume 命令，使用指定的 afrog-resume.cfg 文件恢复扫描

#### [v2.8.9] - 2023-10-15

**新增**  
- 新增 -dingtalk 命令用于 Dingtalk webhook  

**优化**  
- Sqlite 入库错误重试功能，最大重试 5 次  
- PoC：weblogic-panel、weblogic-weak-login  

**删除**  
- PoC：backup-files

<!--auto_detail_active_end_f9cf7911015e9913b7e691a7a5878527-->
