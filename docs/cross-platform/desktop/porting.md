---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: 데스크톱 앱 포팅 지침
description: 기존 Windows Forms 또는 WPF 앱을 분리 하 여 macOS, iOS, Android 및 UWP/Windows 10에서 실행 되는 플랫폼 간 앱을 만드는 방법에 대 한 간단한 설명입니다.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 181034a4936b2da010a2fcd280ded1a3419d43ae
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016445"
---
# <a name="desktop-app-porting-guidance"></a>데스크톱 앱 포팅 지침

대부분의 응용 프로그램 코드는 다음 영역 중 하나로 분류 될 수 있습니다.

- 사용자 인터페이스 코드 (예: windows 및 단추)
- 타사 컨트롤 (예: 그래프
- 비즈니스 논리 (예: 유효성 검사 규칙)
- 로컬 데이터 저장소 및 액세스
- 웹 서비스 및 원격 데이터 액세스

(또는 Visual Basic.NET)로 C# 작성 된 WINDOWS FORMS 및 WPF 응용 프로그램의 경우 많은 비즈니스 논리, 로컬 데이터 액세스 및 웹 서비스 코드를 플랫폼 간에 공유할 수 있습니다.

## <a name="net-portability-analyzer"></a>.NET 이식성 분석기

Visual Studio 2017 이상에서는 기존 응용 프로그램을 검사 하 고 다른 플랫폼에 "있는 그대로" 이식할 수 있는 코드의 양을 알 수 있는 [.Net 이식성 분석기](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([Windows 용 다운로드](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer))를 지원 합니다. 이 [채널 9 비디오](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)에서 자세히 알아볼 수 있습니다.

또한 [GitHub의 이식성 분석기](https://github.com/Microsoft/dotnet-apiport) 에서 명령줄 도구를 다운로드 하 여 동일한 보고서를 제공 하는 데 사용할 수 있습니다.

## <a name="x-of-my-code-is-portable-what-next"></a>"내 코드의 x%를 이식 가능 하 게 합니다. 다음은 무엇 인가요? "

분석기에서 코드의 상당 부분을 이식 가능 하 게 표시 하는 것이 좋습니다. 하지만 다른 플랫폼으로 이동할 _수_ 없는 모든 앱의 일부가 될 것입니다.

코드의 여러 청크는 이러한 버킷 중 하나에 포함 될 수 있으며, 아래에 자세히 설명 되어 있습니다.

- 재사용 가능한 이식 가능 코드
- 변경 해야 하는 코드
- 이식 가능 하지 않고 다시 작성 해야 하는 코드

### <a name="re-useable-portable-code"></a>재사용 가능한 이식 가능 코드

모든 플랫폼에서 사용할 수 있는 Api에 대해 작성 된 .NET 코드는 변경 되지 않은 플랫폼 간으로 가져올 수 있습니다. 이상적으로이 코드를 이식 가능한 클래스 라이브러리, 공유 라이브러리 또는 .NET Standard 라이브러리로 이동한 다음 기존 앱 내에서 테스트할 수 있습니다.

그런 다음 다른 플랫폼 (예: Android, iOS, macOS)의 응용 프로그램 프로젝트에 해당 공유 라이브러리를 추가할 수 있습니다.

### <a name="code-that-requires-changes"></a>변경 해야 하는 코드

일부 .NET Api는 일부 플랫폼에서 사용 하지 못할 수 있습니다. 이러한 Api가 코드에 있는 경우 플랫폼 간 Api를 사용 하려면 해당 섹션을 다시 작성 해야 합니다.

이러한 예로 .NET 4.6에서 사용할 수 있지만 모든 플랫폼에서 사용할 수 없는 리플렉션 Api를 사용 하는 경우를 들 수 있습니다.

이식 가능한 Api를 사용 하 여 코드를 다시 작성 한 후에는 해당 코드를 공유 라이브러리에 패키지 하 고 기존 앱 내에서 테스트할 수 있어야 합니다.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>이식 가능 하지 않고 다시 작성 해야 하는 코드

플랫폼 간이 아닌 코드의 예는 다음과 같습니다.

- 예를 들어 Android 또는 iOS의 프로젝트에서는 **사용자 인터페이스** – WINDOWS FORMS 또는 WPF 화면을 사용할 수 없습니다. 이 [컨트롤 비교](~/cross-platform/desktop/controls/index.md) 를 참조로 사용 하 여 사용자 인터페이스를 다시 작성 해야 합니다.

- 플랫폼별 **저장소** -플랫폼 특정 기술 (예: 로컬 SQL Server Express 데이터베이스)을 사용 하는 코드입니다. 플랫폼 간 대체 (예: 데이터베이스 엔진의 경우 SQLite)를 사용 하 여이를 다시 작성 해야 합니다.
UWP에는 Android 및 iOS에 대해 약간 다른 Api가 있기 때문에 일부 파일 시스템 작업을 조정 해야 할 수도 있습니다 (예: 일부 파일 시스템은 대/소문자를 구분 하 고 다른 일부는 그렇지 않습니다.

- 타사 **구성 요소** – 응용 프로그램의 타사 구성 요소를 다른 플랫폼에서 사용할 수 있는지 여부를 확인 합니다. 비시각적 NuGet 패키지와 같은 일부는 사용 가능 하지만 다른 사용자 (특히 차트나 미디어 플레이어와 같은 시각적 컨트롤)는 사용할 수 있습니다.

## <a name="tips-for-making-code-portable"></a>코드를 이식 가능 하 게 만들기 위한 팁

- **종속성 주입** – 각 플랫폼에 대해 서로 다른 구현을 제공 합니다.

- **계층화 된 방법** – MVVM, MVC, MVP 또는 플랫폼 특정 코드에서 이식 가능한 코드를 분리 하는 데 도움이 되는 다른 패턴이 있는지 여부입니다.

- **메시징** – 코드에서 메시지 전달을 사용 하 여 응용 프로그램의 서로 다른 부분 간의 상호 작용을 제거할 수 있습니다.
