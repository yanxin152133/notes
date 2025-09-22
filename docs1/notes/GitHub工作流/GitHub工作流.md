# 1. GitHub 工作流
>GitHub Actions 是一种持续集成和持续交付 (CI/CD) 平台，可用于自动执行生成、测试和部署管道。 您可以创建工作流程来构建和测试存储库的每个拉取请求，或将合并的拉取请求部署到生产环境。

## 1.1. 组件
### 1.1.1. Workflows 工作流
工作流程是一个可配置的自动化过程，它将运行一个或多个作业。 工作流程由签入到存储库的 YAML 文件定义，并在存储库中的事件触发时运行，也可以手动触发，或按定义的时间表触发。

workflow 文件必须存储在仓库的 `.github/workflows` 的目录中，扩展名为 `.yml` 或 `.yaml`。存储库可以有多个工作流程，每个工作流程都可以执行不同的任务集。

### 1.1.2. Events 事件
事件是存储库中触发工作流程运行的特定活动。例如，向存储库提交`pr 或 pull `请求。

### 1.1.3. Jobs 作业
作业是工作流中在同一运行器上执行的一组步骤。 每个步骤要么是一个将要执行的 shell 脚本，要么是一个将要运行的动作。 步骤按顺序执行，并且相互依赖。 由于每个步骤都在同一运行器上执行，因此您可以将数据从一个步骤共享到另一个步骤。 例如，可以有一个生成应用程序的步骤，后跟一个测试已生成应用程序的步骤。

可以配置作业与其他作业的依赖关系；默认情况下，作业没有依赖关系，并且彼此并行运行。 当一个作业依赖于另一个作业时，它将等待从属作业完成，然后才能运行。 例如，对于没有依赖关系的不同体系结构，您可能有多个生成作业，以及一个依赖于这些作业的打包作业。 生成作业将并行运行，当它们全部成功完成后，打包作业将运行。

### 1.1.4. Steps 步骤
可以在作业中运行命令的单个任务。步骤可以是操作，也可以是 shell 命令。作业中的每个步骤都在同一个运行程序上执行，从而允许该作业中的操作彼此共享数据。

### 1.1.5. Actions 操作
操作是用于 GitHub Actions 平台的自定义应用程序，它执行复杂但经常重复的任务。 使用操作可帮助减少在工作流程文件中编写的重复代码量。操作是工作流中最小的可移植构建块。

### 1.1.6. Runners 运行程序
运行程序是触发工作流时运行工作流的服务器。 每个运行器一次可以运行一个作业。GitHub 提供 Ubuntu Linux、Microsoft Windows 和 macOS 运行器来运行您的工作流程；每个工作流程运行都在新预配的全新虚拟机中执行。 GitHub 还提供 大型运行器（适用于大型配置）。

## 1.2. workflow 文件
### 1.2.1. name
`name`字段是 `workflow` 的名称。如果省略该字段，默认为`当前 workflow 的文件名`。

```yaml
name: GitHub Actions Demo
```

### 1.2.2. on
`on`字段指定`触发 workflow `的条件，通常是某些事件。

```yaml
on: push    ## push事件触发 workflow
```

`on`字段也可以是`事件的数组`。

```yaml
on: [push, pull_request]    ## push事件或pull_request事件都可以触发 workflow
```

### 1.2.3. on.<push|pull_request>.<tags|branches>
指定触发事件时，可以限定分支或标签。

```yaml
on:
  push:
    branches:
      - master    ## 只有master分支发生push事件时，才会触发 workflow
```

### 1.2.4. jobs.<job_id>.name
`jobs`字段里面，需要写出每一项任务的`job_id`，具体名称`自定义`。`job_id`里面的`name`字段是`任务的说明`。

```yaml
jobs:
  my_first_job:    ## job_id为my_first_job
    name: My first job
  my_second_job:    ## job_id为my_second_job
    name: My second job
```

### 1.2.5. jobs.<job_id>.needs
`needs`字段指定当前任务的依赖关系，即`运行顺序`。

```yaml
jobs:
  job1:
  job2:
    needs: job1    ## job1必须先于job2完成
  job3:
    needs: [job1, job2]    ## job3等待job1和job2的完成才能运行
```

### 1.2.6. jobs.<job_id>.runs-on
`runs-on`字段指定运行所需要的`虚拟机环境`。它是`必填字段`。目前可用的虚拟机如下:

```yaml
ubuntu-latest，ubuntu-18.04或ubuntu-16.04
windows-latest，windows-2019或windows-2016
macOS-latest或macOS-10.14
```

### 1.2.7. jobs.<job_id>.steps
`steps`字段指定每个 `Job` 的`运行步骤`，可以包含`一个或多个`步骤。每个步骤都可以指定以下三个字段。

```yaml
jobs.<job_id>.steps.name：步骤名称。
jobs.<job_id>.steps.run：该步骤运行的命令或者 action。
jobs.<job_id>.steps.env：该步骤所需的环境变量。
```