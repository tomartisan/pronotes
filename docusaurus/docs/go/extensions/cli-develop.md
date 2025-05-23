# cli-develop

> 通过一款命令行工具的开发，学习 Golang 的基本知识

## 来自 GPT 的意见

promote：你是一位经验丰富的全栈开发者，对 golang,python,nodejs 等都有丰富的实践经验

### 第一轮问答

- 问：我现在想开发一个在 macOS 系统下的控制台命令工具，能够实现翻译功能。你有什么好的最佳实践方案吗？ 或者推荐哪种编程语言或技术栈呢？
- 答：开发一个在 macOS 系统下的控制台命令工具来实现翻译功能，有几种不同的方案可以选择。以下是几种可能的编程语言和技术栈，以及一些最佳实践建议：

#### 选择编程语言和技术栈

1. **Python**

   - **优点**: 简单易学，丰富的库和框架，广泛的社区支持。
   - **库**: 可以使用 `argparse` 解析命令行参数，使用 `requests` 库进行网络请求，使用 Google Translate API 或其他翻译 API 进行翻译。

2. **Golang**

   - **优点**: 编译成独立的二进制文件，性能高，适合系统工具开发。
   - **库**: 可以使用 `cobra` 或 `flag` 库解析命令行参数，使用 `net/http` 进行网络请求。

3. **Node.js**
   - **优点**: 异步处理能力强，npm 有丰富的包管理系统，可以快速搭建。
   - **库**: 可以使用 `commander` 解析命令行参数，使用 `axios` 库进行网络请求。

#### 最佳实践建议

1. **错误处理**: 确保你处理了网络请求失败、API 限制以及其他可能的错误情况，并提供用户友好的错误信息。
2. **环境变量**: API 密钥等敏感信息不要硬编码在代码中，使用环境变量或配置文件来管理。
3. **用户体验**: 提供清晰的帮助信息和文档，让用户能够快速上手使用工具。
4. **单元测试**: 为你的代码编写单元测试，确保每个功能模块都能正常工作。
5. **版本管理**: 使用版本管理工具（如 Git）来管理代码变更，并提供清晰的版本发布日志。

选择具体的技术栈可以根据你的个人偏好和项目需求来决定。如果你希望工具能够快速开发并且易于维护，Python 是一个不错的选择。如果你更关注性能和二进制分发，Golang 则是一个好选择。Node.js 则适合那些需要异步处理或者已经有丰富 JavaScript 经验的开发者。

### 第二轮问答

- 问：如果要提交到 homebrew，可以让别的用户方便的通过：brew install 安装使用。在这个情况下，用哪种编程语言更合适呢？
- 答：为了提交到 Homebrew 并让其他用户方便地通过 brew install 来安装和使用，选择哪种编程语言主要取决于几个因素：程序的部署和打包难度、依赖管理以及你的个人偏好。以下是每种编程语言的优缺点，具体适用于 Homebrew 的情况：

为了提交到 Homebrew 并让其他用户方便地通过`brew install`来安装和使用，选择哪种编程语言主要取决于几个因素：程序的部署和打包难度、依赖管理以及你的个人偏好。以下是每种编程语言的优缺点，具体适用于 Homebrew 的情况：

#### Python

**优点**:

- 简单易学，开发速度快。
- 丰富的第三方库。
- 跨平台支持好。

**缺点**:

- 需要依赖 Python 解释器和相关库，可能增加依赖管理的复杂性。

**打包和部署**:

- 可以使用 `pyinstaller` 或 `pex` 将 Python 代码打包成独立的可执行文件，以减少对用户环境的依赖。

**示例**:

```shell
# 使用 pyinstaller 打包
pyinstaller --onefile your_script.py

# 然后在 Formula 中使用下载好的可执行文件
```

#### Golang

**优点**:

- 编译成独立的二进制文件，无需额外依赖。
- 性能高，适合系统工具开发。

**缺点**:

- 编译时间可能较长，开发过程可能不如 Python 灵活。

**打包和部署**:

- 编译成单独的二进制文件，直接在 Homebrew 中引用。

**示例**:

```shell
# 编译二进制文件
go build -o your_tool main.go

# 然后在 Formula 中使用下载好的可执行文件
```

#### Node.js

**优点**:

- 异步处理能力强，适合 I/O 密集型任务。
- 丰富的 npm 包管理系统。

**缺点**:

- 需要 Node.js 运行时环境和相关 npm 包，增加了依赖管理的复杂性。

**打包和部署**:

- 可以使用 `pkg` 将 Node.js 代码打包成独立的可执行文件。

**示例**:

```shell
# 使用 pkg 打包
pkg your_script.js

# 然后在 Formula 中使用下载好的可执行文件
```

#### 综合建议

对于提交到 Homebrew 的命令行工具，Golang 通常是最佳选择。它的编译特性使其生成的二进制文件可以在没有任何依赖的情况下运行，非常适合 Homebrew 的分发模式。

#### Homebrew Formula 示例

以下是一个如何为 Golang 编写的工具创建 Homebrew Formula 的示例：

```ruby
class YourTool < Formula
  desc "Your tool description"
  homepage "https://example.com"
  url "https://github.com/yourusername/yourrepository/archive/v1.0.0.tar.gz"
  sha256 "your_tarball_checksum"

  depends_on "go" => :build

  def install
    system "go", "build", "-o", "#{bin}/your_tool", "."
  end

  test do
    assert_match "Expected output", shell_output("#{bin}/your_tool --version")
  end
end
```

在这个 Formula 中，`go build`命令被用来编译工具，并将其安装到 Homebrew 的 bin 目录中。你可以在测试中加入一些简单的命令来验证安装是否成功。

### 第三轮问答

- 问：明白了！ 那对于 golang，有没有类似的脚手架工具可供参考或使用呢？
- 答：如下

在 Golang 中，有一些优秀的脚手架工具和框架可以帮助你快速搭建项目结构，处理命令行参数，以及管理项目依赖。以下是一些常用的工具和库：

#### 1. Cobra

Cobra 是一个用于创建命令行应用程序的库，可以帮助你快速构建复杂的 CLI 工具。

**特点**:

- 强大的命令和子命令管理
- 易于使用的标志和配置
- 丰富的文档和社区支持

**安装**:

```shell
go get -u github.com/spf13/cobra/cobra
```

**使用**:
创建一个新的 Cobra 项目：

```shell
cobra init --pkg-name github.com/yourusername/yourproject
cd yourproject
cobra add translate
```

**示例代码**:

```go
package main

import (
    "fmt"
    "github.com/spf13/cobra"
)

var rootCmd = &cobra.Command{
    Use:   "yourtool",
    Short: "A brief description of your tool",
    Long:  `A longer description of your tool`,
    Run: func(cmd *cobra.Command, args []string) {
        // Your main code here
    },
}

var translateCmd = &cobra.Command{
    Use:   "translate",
    Short: "Translate text using Google Translate API",
    Long:  `Translate text using Google Translate API`,
    Run: func(cmd *cobra.Command, args []string) {
        // Your translation code here
    },
}

func main() {
    rootCmd.AddCommand(translateCmd)
    if err := rootCmd.Execute(); err != nil {
        fmt.Println(err)
        os.Exit(1)
    }
}
```

#### 2. Go Project Layout

Go Project Layout 是一个推荐的项目结构，可以作为你的项目基础。虽然不是严格意义上的脚手架工具，但它提供了一种标准化的项目布局。

**项目结构**:

```plaintext
.
├── cmd
│   └── yourtool
│       └── main.go
├── pkg
│   └── translate
│       └── translate.go
├── internal
│   └── some_internal_package
│       └── internal.go
├── go.mod
├── go.sum
└── README.md
```

#### 3. Viper

Viper 是一个用于配置管理的库，通常与 Cobra 一起使用，可以帮助你处理配置文件、环境变量等。

**安装**:

```shell
go get -u github.com/spf13/viper
```

**使用**:

```go
import (
    "github.com/spf13/viper"
)

func initConfig() {
    viper.SetConfigName("config")
    viper.SetConfigType("yaml")
    viper.AddConfigPath(".")
    err := viper.ReadInConfig()
    if err != nil {
        fmt.Println("Error reading config file", err)
    }
}

func main() {
    initConfig()
    // Use viper.Get("key") to get configuration values
}
```

#### 4. GoReleaser

GoReleaser 是一个用于构建和发布 Go 项目的工具，可以帮助你自动化构建、打包和发布流程。

**安装**:

```shell
brew install goreleaser/tap/goreleaser
```

**配置**:
创建一个 `.goreleaser.yml` 文件：

```yaml
project_name: yourtool
builds:
  - main: ./cmd/yourtool
    goos:
      - darwin
      - linux
      - windows
archives:
  - format: tar.gz
    name_template: "{{ .ProjectName }}_{{ .Version }}_{{ .Os }}_{{ .Arch }}"
release:
  github:
    owner: yourusername
    name: yourrepository
```

**使用**:

```shell
goreleaser release --rm-dist
```

结合 Cobra、Viper 和 GoReleaser，你可以快速搭建一个功能齐全、配置灵活的 Go 项目，并轻松地进行发布和分发。这些工具的结合可以显著提高你的开发效率和项目管理的规范性。

#### 第四轮问答

- 问：那辛苦你帮我在 github 上找找类似的开源项目，我做个参考
- 答：如下

Here are some examples of open-source projects using Golang and Cobra to create CLI tools, which can serve as references for your own project:

1. **Cobra CLI**: This project provides a powerful CLI framework and a tool to generate applications and commands. It offers a comprehensive structure for building CLI applications and is widely used in the Go community. The repository includes detailed instructions on initializing a new project and adding commands.

   - [GitHub - Cobra CLI](https://github.com/spf13/cobra-cli) [oai_citation:1,GitHub - spf13/cobra-cli: Cobra CLI tool to generate applications and commands](https://github.com/spf13/cobra-cli).

2. **Basic Cobra CLI**: This is a minimal example of a CLI tool built with Golang and Cobra. It includes a basic setup and demonstrates how to create nested commands.

   - [GitHub - Basic Cobra CLI](https://github.com/frasnym/basic-cobra-cli) [oai_citation:2,GitHub - frasnym/basic-cobra-cli: A Powerful CLI Tool Built with Golang and Cobra](https://github.com/frasnym/basic-cobra-cli).

3. **Cobra Example**: This project is a minimal example of a Cobra application, showcasing how to initialize a new application, add commands, and structure the CLI tool.

   - [GitHub - Cobra Example](https://github.com/dharmeshkakadia/cobra-example) [oai_citation:3,GitHub - dharmeshkakadia/cobra-example: minimal example of cobra app](https://github.com/dharmeshkakadia/cobra-example).

4. **Cobra Viper Demo**: This example demonstrates how to use Cobra in conjunction with Viper, another popular library for configuration management in Golang. It illustrates how to create a CLI interface with configuration file support.
   - [GitHub - Cobra Viper Demo](https://github.com/northbright/cobra-viper-demo) [oai_citation:4,GitHub - northbright/cobra-viper-demo: Example of using cobra and viper to create CLI interfaces for Golang app](https://github.com/northbright/cobra-viper-demo).

These repositories should provide a solid foundation and best practices for developing your own CLI tool using Golang and Cobra.
