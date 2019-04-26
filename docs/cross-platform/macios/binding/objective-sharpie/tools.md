---
title: 목표 Sharpie 도구 및 명령
description: 이 문서에서는 목표 Sharpie 및 명령줄 인수를 하는 데 사용 하 여 포함 된 도구 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 718b5104ddc4593d080b88b062c42d371d9e8e2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261170"
---
# <a name="objective-sharpie-tools--commands"></a>목표 Sharpie 도구 및 명령

_개요와 목표 Sharpie 명령줄 인수를 사용 하는 포함 된 도구입니다._

<style type="text/css"> .terminal-blue { color: rgb(10,96,254); } .terminal-green { color: rgb(12,156,26); } .terminal-magenta { color: rgb(152,12,103); } </style>


목표 Sharpie 성공적으로 되 면 [설치](~/cross-platform/macios/binding/objective-sharpie/get-started.md)터미널을 열고 숙지 합니다 <em>명령</em> 목표 Sharpie에서 제공 하는:

<pre>$ <b>sharpie -help</b>
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
  docs               Open the Objective Sharpie online documentation</pre>

목표 Sharpie 다음 도구를 제공합니다.

|도구|설명|
|--- |--- |
|**xcode**|현재 설치 된 Xcode 및 iOS 및 사용할 수 있는 Mac Sdk의 버전에 대 한 정보를 제공 합니다. 이 바인딩을 생성 하는 경우 나중에이 정보를 사용 합니다.|
|**pod**|에 대 한 검색, 구성 하 고, (로컬 디렉터리)에 설치 하 고 Objective-c로 바인딩합니다 [CocoaPod](https://cocoapods.org/) 마스터 사양 리포지토리에서 사용할 수 있는 라이브러리입니다. 이 도구에 전달할 입력이 올바른지를 자동으로 추론에 설치 된 CocoaPod 평가 `bind` 아래 도구입니다. 3.0의 새로운 기능!|
|**bind**|헤더 파일을 구문 분석 (`*.h`)에서 초기에 Objective-c 라이브러리 [ApiDefinition.cs 및 StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) 파일입니다.|
|**update**|목표 Sharpie의 최신 버전을 확인 하 고 다운로드 하 고 사용 가능한 경우 설치 관리자를 시작 합니다.|
|**verify-docs**|에 대 한 자세한 정보 표시 `[Verify]` 특성입니다.|
|**docs**|기본 웹 브라우저에서이 문서로 이동합니다.|

특정 목표 Sharpie 도구 도움말을 보려면 도구의 이름을 입력 하며 `-help` 옵션입니다. 예를 들어 `sharpie xcode -help` 다음 출력을 반환 합니다.

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

터미널에 다음 명령을 입력 하 여 현재 설치 된 Sdk에 대 한 정보를 얻을 수 해야 바인딩 프로세스를 시작할 수 있습니다, 전에 `sharpie xcode -sdks`입니다. 출력은 설치한 Xcode의 버전에 따라 달라질 수 있습니다. Sdk에서 설치에 대 한 목표 Sharpie 찾습니다 `Xcode*.app` 아래는 `/Applications` 디렉터리:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

위의 보면 있다는 것을 `iphoneos9.1` SDK 컴퓨터에 설치 되어 있고 `arm64` 아키텍처 지원 합니다. 이 섹션에 있는 모든 샘플에 대 한이 값이를 사용 합니다. 이 정보를 사용 하 여 초기에는 Objective C 라이브러리 헤더 파일을 구문 분석할 준비가 됩니다 `ApiDefinition.cs` 고 `StructsAndEnums.cs` 바인딩 프로젝트에 대 한 합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin University 과정: Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie로는 Objective-c 바인딩 라이브러리 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)