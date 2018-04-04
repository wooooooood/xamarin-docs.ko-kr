---
title: CocoaPods를 사용 하 여 실제 예제
description: 이 문서에서에서 자동으로 생성할 C# 바인딩 정의 CocoaPod 목표 Sharpie를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: cbcafc8d77304d117f8130cf0d6a89dd2a5ed3c8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods를 사용 하 여 실제 예제

> [!NOTE]
> 사용 하 여이 예제는 [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking)합니다.

버전 3.0의 새로운 목표 Sharpie CocoaPods를 바인딩하는 지원 한도 명령이 포함 되어 (`sharpie pod`) 다운로드, 구성 및 CocoaPods 매우 쉽게 작성할 수 있도록 합니다. 수행 해야 [CocoaPods를 숙지](https://cocoapods.org) 이 기능을 사용 하기 전에 일반적입니다.

## <a name="creating-a-binding-for-a-cocoapod"></a>CocoaPod에 대 한 바인딩 만들기

`sharpie pod` 명령에는 하나의 전역 옵션 및 두 개의 하위 명령:

```bash
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

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

여러 개의 CocoaPod 이름과 subspec 이름에 제공 될 수 `init`합니다.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

프로그램 CocoaPod로 설정 된, 일단 바인딩을 지금 만들 수 있습니다.

```bash
$ sharpie pod bind
```

그러면 CocoaPod Xcode 프로젝트 되 고 작성 된 다음 평가 하 고 목표 Sharpie에서 구문 분석 됩니다. 콘솔 출력을 많이 생성 됩니다 이지만 끝에 바인딩 정의 발생 해야 합니다.

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>다음 단계

생성 한 후의 **ApiDefinitions.cs** 및 **StructsAndEnums.cs** 파일을 앱에서 사용할 어셈블리를 생성 하는 다음 문서에 대해 살펴봅니다.

- [바인딩 Objective-c 개요](~/cross-platform/macios/binding/overview.md)
- [바인딩 Objective C 라이브러리](~/cross-platform/macios/binding/objective-c-libraries.md)
- [연습: 바인딩 iOS Objective C 라이브러리](~/ios/platform/binding-objective-c/walkthrough.md)

