---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: 데스크톱 앱 포팅 참고 자료
description: UWP 또는 Windows 10 뿐만 아니라 macOS, iOS, Android에서 실행 하는 플랫폼 간 앱을 만들려면 기존 Windows Forms 또는 WPF 앱을 분리 하는 방법의 간단한 설명입니다.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4bf1dea170bd6b63209693963d54cc2e16163eea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61270029"
---
# <a name="desktop-app-porting-guidance"></a>데스크톱 앱 포팅 참고 자료

대부분의 응용 프로그램 코드는 다음 영역 중 하나로 분류할 수 있습니다.

* 사용자 인터페이스 코드 (예: windows 및 단추)
* 타사 컨트롤 (예: 차트)
* 비즈니스 논리 (예: 유효성 검사 규칙)
* 로컬 데이터 저장 및 액세스
* 웹 서비스 및 원격 데이터 액세스

사용 하 여 작성 된 Windows Forms 및 WPF 응용 프로그램에 대 한 C# (또는 Visual Basic.NET) 플랫폼에서 상당히 많은 비즈니스 논리, 로컬 데이터 액세스 및 웹 서비스 코드를 공유할 수 있습니다.

## <a name="net-portability-analyzer"></a>.NET 이식성 분석기

Visual Studio 2017 및 이상 지원 합니다 [.NET Portability Analyzer](https://docs.microsoft.com/dotnet/articles/standard/portability-analyzer) ([Windows 용 다운로드](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer))는 기존 응용 프로그램을 검토 하 고 다른 코드의 양을 "있는 그대로" 이식할 수를 알 수 플랫폼입니다. 이 대 한 자세히 알아볼 수 있습니다 [Channel 9 비디오](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)합니다.

이기도에서 명령줄 도구를 다운로드할 수 있습니다 [GitHub에서 이식성 분석기](https://github.com/Microsoft/dotnet-apiport) 동일한 보고서를 제공 하는 데 사용 합니다.

## <a name="x-of-my-code-is-portable-what-next"></a>"x %의 내 코드를 이식 가능 합니다. 다음은 뭔가요? "

바랍니다 분석기 나타나지만 큰 부분 코드는 이식 가능은 확실히 있습니까 모든 앱의 일부는 _없습니다_ 다른 플랫폼으로 이동 합니다.

코드의 다른 청크 아래에서 자세히 설명, 이러한 버킷 중 하나에 해당 아마도 됩니다.

* 이식 가능한 코드를 다시 사용
* 변경 해야 하는 코드
* 이식 가능한 아니며 다시 작성 해야 하는 코드

### <a name="re-useable-portable-code"></a>이식 가능한 코드를 다시 사용

모든 플랫폼에서 사용할 수 있는 Api에 대해 기록 되는.NET 코드는 플랫폼 간 변경 하지 않고 수행할 수 있습니다. 이상적으로이 모든 코드를 이식 가능한 클래스 라이브러리, 라이브러리 공유 또는.NET Standard 라이브러리를 이동 하 고 다음 기존 앱 내에서 테스트를 수 있습니다.

공유 라이브러리 (예: Android, iOS, macOS) 다른 플랫폼용 응용 프로그램 프로젝트에 추가할 수 있습니다.

### <a name="code-that-requires-changes"></a>변경 해야 하는 코드

모든 플랫폼에서 일부.NET Api을 사용 하지 못할 수도 있습니다. 이러한 Api 코드에 존재 하는 경우 플랫폼 간 Api 사용 하 여 해당 섹션을 다시 작성 해야 합니다.

이 예가.NET 4.6에서 사용할 수 있지만 모든 플랫폼에서 사용할 수 없는 리플렉션 Api를 사용 합니다.

이식 가능한 Api를 사용 하는 코드를 다시 작성 한 후에 코드 공유 라이브러리에 패키지를 기존 앱 내에서 테스트 수 있어야 합니다.

### <a name="code-that-isnt-portable-and-requires-a-re-write"></a>이식 가능한 아니며 다시 작성 해야 하는 코드

플랫폼 간 일 수 없는 코드의 예입니다.

- **사용자 인터페이스** – Windows Forms 또는 WPF 화면에서 Android 또는 iOS 프로젝트에서 예를 들어 사용할 수 없습니다. 이 사용 하 여 사용자 인터페이스를 다시 작성 해야 합니다 [컨트롤 비교](~/cross-platform/desktop/controls/index.md) 참조로 합니다.

- **플랫폼별 저장소** -플랫폼 특정 기술 (예: SQL Server Express 로컬 데이터베이스)에 의존 하는 코드입니다. 다시 작성이 플랫폼 간 대안 (예: 데이터베이스 엔진에 SQLite)를 사용 해야 합니다.
일부 파일 시스템 작업 UWP에 약간 다른 Api (예: iOS 및 Android를 조정 해야 할 수도 일부 파일 시스템은 대/소문자 구분 및 다른 사용자가 아닌).

- **타사 구성 요소** – 타사 구성 요소 응용 프로그램에서 다른 플랫폼에서 사용할 수 있는지 여부를 확인 합니다. 비가 시 NuGet 패키지와 같은 일부 다른 (특히 visual 컨트롤 차트 또는 미디어 플레이어와 같은)에 있지만 사용할 수 있습니다.

## <a name="tips-for-making-code-portable"></a>이식 가능한 코드에 대 한 팁

- **종속성 주입** – 각 플랫폼에 대해 서로 다른 구현을 제공 하 고

- **계층화 된 접근 방식을** – MVVM, MVC, MVP, 또는 도움이 되는 몇 가지 다른 패턴 플랫폼별 코드에서 이식 가능한 코드를 구분 하는지 여부입니다.

- **메시징** – 취소 응용 프로그램의 다른 부분 간의 상호 작용을 결합 하 메시지 전달 코드에서 사용할 수 있습니다.
