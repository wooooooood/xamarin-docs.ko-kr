---
title: CocoaPods를 사용 하는 실제 예제
description: 이 문서에서는 목적 Sharpie를 사용 하 여 CocoaPod에서 C# 바인딩 정의를 자동으로 생성 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: conceptdev
ms.author: crdun
ms.date: 03/28/2018
ms.openlocfilehash: 0f730b1c0a0deacdb84c198cfe4af47308a268cc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290024"
---
# <a name="real-world-example-using-cocoapods"></a>CocoaPods를 사용 하는 실제 예제

> [!NOTE]
> 이 예제에서는 [Afnetworking CocoaPod](https://cocoapods.org/pods/AFNetworking)를 사용 합니다.

버전 3.0의 새로운 기능이 며, 목표 Sharpie는 CocoaPods 바인딩을 지원 하며, CocoaPods를`sharpie pod`매우 쉽게 다운로드, 구성 및 빌드하기 위한 명령 ()도 포함 합니다. 이 기능을 사용 하기 전에 [CocoaPods에 대해 잘 알고](https://cocoapods.org) 있어야 합니다.

## <a name="creating-a-binding-for-a-cocoapod"></a>CocoaPod에 대 한 바인딩 만들기

명령 `sharpie pod` 에는 하나의 전역 옵션과 두 개의 하위 명령이 있습니다.

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

하위 `init` 명령에는 다음과 같은 몇 가지 유용한 도움말이 있습니다.

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

에 `init`는 여러 CocoaPod 이름 및 subspec 이름을 제공할 수 있습니다.

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

CocoaPod가 설정 되 면 이제 바인딩을 만들 수 있습니다.

```bash
$ sharpie pod bind
```

이로 인해 CocoaPod Xcode 프로젝트가 빌드되고 목표 Sharpie를 기준으로 계산 및 구문 분석 됩니다. 많은 콘솔 출력이 생성 되지만 끝에 바인딩 정의가 생성 됩니다.

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>다음 단계

**ApiDefinitions.cs** 및 **StructsAndEnums.cs** 파일을 생성 한 후에는 다음 설명서를 참조 하 여 앱에서 사용할 어셈블리를 생성 합니다.

- [바인딩 목표-C 개요](~/cross-platform/macios/binding/overview.md)
- [바인딩 목표-C 라이브러리](~/cross-platform/macios/binding/objective-c-libraries.md)
- [연습: IOS 목표-C 라이브러리 바인딩](~/ios/platform/binding-objective-c/walkthrough.md)
