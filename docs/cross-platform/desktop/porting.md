---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: 데스크톱 응용 프로그램 포팅 가이드
description: 기존 Windows Forms 또는 WPF 앱 macOS, iOS, Android, 뿐만 아니라 UWP/Windows 10에서 실행 하는 플랫폼 간 앱 만들기를 분리 하는 방법의 간단한 설명 했습니다.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: cdf70065893a4da268f628369fa94336291ead1f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="desktop-app-porting-guidance"></a>데스크톱 응용 프로그램 포팅 가이드

대부분의 응용 프로그램 코드는 다음과 같은 영역 중 하나로 분류할 수 있습니다.

* 사용자 인터페이스 코드 (예:. 창 및 단추)
* 타사 컨트롤 (예:. 차트)
* 비즈니스 논리 (예:. 유효성 검사 규칙)
* 로컬 데이터 저장 및 액세스
* 웹 서비스 및 원격 데이터 액세스

Windows Forms 및 WPF 응용 프로그램의 비즈니스 논리의 상당히 많은 C# (또는 Visual Basic.NET)로 작성 된 경우 로컬 데이터 액세스 및 웹 서비스 코드를 플랫폼 간에 공유할 수 있습니다.

## <a name="net-portability-analyzer"></a>.NET 이식성 분석기

Visual Studio 2015 및 2017 지원은 [.NET 이식성 분석기](https://docs.microsoft.com/en-us/dotnet/articles/standard/portability-analyzer) ([Windows 용 다운로드](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) 기존 응용 프로그램을 검토 하 고 코드에 다른 플랫폼에 "있는 그대로" 이식할 수를 알 수 있는 . 이 항목에 대 한 자세히 알아볼 수 있습니다 [Channel 9 비디오](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)합니다.

있어서 파일에서 명령줄 도구를 다운로드할 수 있습니다 [GitHub의 이식성 분석기](https://github.com/Microsoft/dotnet-apiport) 같은 보고서가 제공 하는 데 사용 합니다.

## <a name="x-of-my-code-is-portable-what-next"></a>"x % 내 코드를 이식할 수 있습니다. "다음 단계

다행히 분석기 나타나지만 큰 코드의 일부는 이식 가능 있습니다 확실히 수 있게 될 모든 앱의 일부는 _없습니다_ 다른 플랫폼으로 이동 합니다.

서로 다른 양의 코드 아래에서 자세히 설명 합니다. 이러한 버킷 중 하나에 해당 아마도 됩니다.

* 재사용이 이식 가능한 코드
* 코드를 변경 해야 합니다.
* 휴대용 없고 다시 작성 해야 하는 코드

### <a name="re-useable-portable-code"></a>재사용이 이식 가능한 코드

플랫폼 간 변경 하지 않고 모든 플랫폼에서 사용할 수 있는 Api에 대해 작성 된.NET 코드를 가져올 수 있습니다. 이상적으로 이식 가능한 클래스 라이브러리, 라이브러리 공유 또는 표준.NET 라이브러리에이 모든 코드를 이동한 다음 기존 앱 내에서 테스트를 수 있습니다.

다음 해당 공유 라이브러리 (Android, iOS, macOS) 등 다른 플랫폼에 대 한 응용 프로그램 프로젝트에 추가할 수 있습니다.

### <a name="code-that-requires-changes"></a>코드를 변경 해야 합니다.

모든 플랫폼에서 일부.NET Api을 사용 하지 못할 수도 있습니다. 이러한 Api 코드에 있으면 해당 섹션 플랫폼 Api를 사용 하도록 다시 작성 해야 합니다.

이 포함의 예는.NET 4.6에서 사용할 수 있지만 모든 플랫폼에서 사용할 수 없는 리플렉션 Api를 활용 합니다.

다시 이식 가능 Api를 사용 하 여 코드를 작성 한 후을 공유 라이브러리에 해당 코드를 패키지 한 기존 앱 내에서 테스트할 수 있습니다.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>휴대용 없고 다시 작성 해야 하는 코드

플랫폼 간 수 없는 코드의 예입니다.

- **사용자 인터페이스** – Windows Forms 또는 WPF 화면에서 Android 또는 iOS 프로젝트에서 예를 사용할 수 없습니다. 이 사용 하 여 사용자 인터페이스를 다시 작성 해야 합니다 [컨트롤 비교](~/cross-platform/desktop/controls/index.md) 를 참조 합니다.

- **플랫폼 특정 저장소** -플랫폼 관련 기술 (예: SQL Server Express 로컬 데이터베이스)를 사용 하는 코드입니다. 다시 작성이 플랫폼 간 좋은 (예: 데이터베이스 엔진에 대 한 SQLite)를 사용 하 여 선택 해야 합니다.
일부 파일 시스템 작업 UWP 않음 (예: iOS 및 Android를 약간 다른 Api가 이기 때문에 조정 해야 할 수도 일부 파일 시스템은 대/소문자 구분 및 다른 사용자가 아닌).

- **3 번째 구성 요소** -3rd 파티 구성 요소 응용 프로그램에서 다른 플랫폼에서 사용할 수 있는지 여부를 확인 합니다. 비시각적 NuGet 패키지와 같은 일부 다른 (특히 visual 제어 차트 또는 미디어 플레이어와 같은) 있지만 사용할 수 있습니다.

## <a name="tips-for-making-code-portable"></a>이식 가능한 코드를 만들기 위한 팁

- **종속성 주입** – 각 플랫폼에 대 한 다른 구현을 제공 하 고

- **계층화 된 접근 방식을** MVVM, MVC, MVP, 또는 사용 하면 일부 다른 패턴 플랫폼별 코드에서 이식 가능한 코드를 구분 하는지 여부를 – 합니다.

- **메시징** – 취소 응용 프로그램의 다른 부분 간의 상호 작용을 결합 하 여 코드에서 메시지 전달을 사용할 수 있습니다.
