---
title: "CocoaPods를 사용 하 여 실제 예제"
ms.topic: article
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ae92b491e6186371f1fc1ead835f918a94f18f86
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods를 사용 하 여 실제 예제


**사용 하 여이 예제는 [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking)합니다.**

버전 3.0의 새로운 목표 Sharpie CocoaPods를 바인딩하는 지원 및에 프런트 엔드 명령이 (`sharpie pod`) 다운로드, 구성 및 CocoaPods 매우 쉽게 작성할 수 있도록 합니다. 수행 해야 [faimilarize 보세요 CocoaPods](https://cocoapods.org) 이 기능을 사용 하기 전에 일반적입니다.

`sharpie pod` 명령에는 하나의 전역 옵션 및 두 개의 하위 명령:

```csharp
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` 하위 명령에 몇 가지 유용한 도움말:

```csharp
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

여러 개의 CocoaPod 이름과 subspec 이름에 제공 될 수 `init`합니다.

<pre>$ <b>sharpie pod init ios AFNetworking</b>
<span class="terminal-green">**</span> Setting up CocoaPods master repo ...
   (this may take a while the first time)
<span class="terminal-green">**</span> Searching for requested CocoaPods ...
<span class="terminal-green">**</span> Working directory:
<span class="terminal-green">**</span>   - Writing Podfile ...
<span class="terminal-green">**</span>   - Installing CocoaPods ...
<span class="terminal-green">**</span>     (running `<span class="terminal-blue">pod install --no-integrate --no-repo-update</span>`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
<span class="terminal-green">**</span> 🍻 Success! You can now use other `<span class="terminal-green">sharpie pod</span>`  commands.</pre>

프로그램 CocoaPod로 설정 된, 일단 바인딩을 지금 만들 수 있습니다.

<pre>$ <b>sharpie pod bind</b></pre>

그러면 CocoaPod Xcode 프로젝트 되 고 작성 된 다음 평가 하 고 목표 Sharpie에서 구문 분석 됩니다. 콘솔 출력을 많이 생성 됩니다 이지만 끝에 바인딩 정의 발생 해야 합니다.

<pre><em>(... lots of build output ...)</em>

<span class="terminal-blue">Parsing 19 header files...</span>

<span class="terminal-magenta">Binding...</span>
  <span class="terminal-magenta">[write]</span> ApiDefinitions.cs
  <span class="terminal-magenta">[write]</span> StructsAndEnums.cs

<span class="terminal-green">Done.</span></pre>

