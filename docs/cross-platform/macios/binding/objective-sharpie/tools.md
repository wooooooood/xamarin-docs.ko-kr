---
title: 목적 Sharpie 도구 & 명령
description: 이 문서에서는 Sharpie에 포함 된 도구에 대 한 개요와 함께 사용할 수 있는 명령줄 인수를 제공 합니다.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: ddfe0f99991808214a6006c9504d267179adf2ab
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521863"
---
# <a name="objective-sharpie-tools--commands"></a>목적 Sharpie 도구 & 명령

_목표 Sharpie 포함 된 도구 개요와 사용 하기 위한 명령줄 인수입니다._

목표 Sharpie 성공적으로 [설치](~/cross-platform/macios/binding/objective-sharpie/get-started.md)되 면 터미널을 열고 Sharpie가 제공 해야 하는 *명령* 목표를 숙지 합니다.

```
$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation
```

목표 Sharpie 다음과 같은 도구를 제공 합니다.

|도구|Description|
|--- |--- |
|**xcode**|현재 Xcode 설치 및 사용할 수 있는 iOS 및 Mac Sdk 버전에 대 한 정보를 제공 합니다. 나중에 바인딩을 생성할 때이 정보를 사용할 예정입니다.|
|**pod**|로컬 디렉터리에서를 검색, 구성 및 설치 하 고 마스터 사양 리포지토리에서 사용 가능한 목표-C [CocoaPod](https://cocoapods.org/) 라이브러리를 바인딩합니다. 이 도구는 설치 된 CocoaPod를 평가 하 여 아래 `bind` 도구에 전달할 올바른 입력을 자동으로 추론 합니다. 3\.0의 새로운|
|**bind**|목적-C 라이브러리의`*.h`헤더 파일 ()을 초기 [ApiDefinition.cs 및 StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 파일로 구문 분석 합니다.|
|**update**|최신 버전의 목표가 Sharpie을 확인 하 고 다운로드 한 후 설치 관리자를 시작 합니다.|
|**verify-docs**|특성에 대 한 `[Verify]` 자세한 정보를 표시 합니다.|
|**docs**|기본 웹 브라우저에서이 문서를 탐색 합니다.|

특정 목표 Sharpie 도구에 대 한 도움말을 보려면 도구 이름 및 `-help` 옵션을 입력 합니다. 예를 들어 `sharpie xcode -help` 는 다음 출력을 반환 합니다.

```
$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.
```

바인딩 프로세스를 시작 하기 전에 터미널 `sharpie xcode -sdks`에 다음 명령을 입력 하 여 현재 설치 된 sdk에 대 한 정보를 가져와야 합니다. 출력은 설치한 Xcode 버전에 따라 다를 수 있습니다. 목표 Sharpie `Xcode*.app` `/Applications` 디렉터리 아래에 설치 된 sdk를 찾습니다.

```
$ sharpie xcode -sdks
sdk: appletvos9.0    arch: arm64
sdk: iphoneos9.1     arch: arm64   armv7
sdk: iphoneos9.0     arch: arm64   armv7
sdk: iphoneos8.4     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: macosx10.10     arch: x86_64  i386
sdk: watchos2.0      arch: armv7
```

위의 항목에서 `iphoneos9.1` SDK가 컴퓨터에 설치 되어 있고 `arm64` 아키텍처를 지원 하는지 확인할 수 있습니다. 이 섹션의 모든 샘플에 대해이 값을 사용 합니다. 이 정보를 사용 하는 경우 목표-C 라이브러리 헤더 파일을 초기 `ApiDefinition.cs` 및 `StructsAndEnums.cs` 바인딩 프로젝트에 대 한 구문 분석할 준비가 된 것입니다.
