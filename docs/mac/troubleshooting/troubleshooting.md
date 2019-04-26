---
title: Xamarin.Mac 문제 해결 팁
description: 이 문서에서는 Xamarin.Mac 응용 프로그램을 개발할 때 발생 하는 문제를 해결 하기 위한 방법을 설명 합니다. 또한 지원을 받는 방법에 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: f498aab5bfaffc08a22f62a318f8f9f73ab0afca
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61237601"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin.Mac 문제 해결 팁

## <a name="overview"></a>개요

경우에 따라 모든 갇 힐을 원하는 방식으로 작업 하기 위한 API를 가져오려는 또는 버그를 해결 하는 동안 프로젝트에서 작업 하는 동안. Xamarin에서 이러한 목표를 성공적으로 모바일 및 데스크톱 응용 프로그램을 작성 하 되며 몇 가지 리소스를 제공 했습니다.

이러한 리소스 중 하나를 사용 하 여 일부의 단계 준비 문제를 신속 하 게 해결할 수 있도록 수행할 수 있습니다.

- 모범 사례로 충돌을 보고 가능한 문제의 근본 원인을 확인 합니다.
 
     - "내 응용 프로그램이 크래시 되" 진단 하기가 어렵습니다. "응용 프로그램이이 호출에는 빈 배열을 반환 하는 경우 충돌"는 해결 하기가 매우 쉽습니다.

     - "작업 NSTable 받을 수 없는"은 "없음 my NSTableDelegate 메서드 보일이 경우에 호출할 수 있습니다." 보다 유용

- 가능한 경우 문제를 보여 주는 작은 예제 프로그램을 제공 합니다. 더 많은 시간과 노력 배나를 사용 하는 문제에 대 한 소스 코드의 페이지를 통해 파기 합니다.

- 새로운 표시 하는 문제가 발생 하는 응용 프로그램에 대 한 변경 내용을 신속 하 게 범위를 좁힐 수 문제의 원인을 파악 합니다. 최근에 Xamarin.Mac, 문제를 일으키는 파트를 찾으려면 응용 프로그램의 섹션 트리밍의 버전을 업그레이드 한 경우 나 변경 내용 문제를 도입 찾으려고 빌드 이전 테스트 하는 것은 매우 유용할 수 있습니다.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>앱 출력 없이 충돌할 때 수행할 작업

대부분의 경우에서 Mac 용 Visual Studio의 디버거 응용 프로그램에서 충돌 및 근본 원인을 추적할 수 있도록 예외를 catch 합니다. 그러나 거의 또는 전혀 출력을 사용 하 여 응용 프로그램은 도킹 스테이션에서 반송 하 고 종료 몇 가지 경우가 있습니다. 포함할 수 있습니다.

- 코드 서명 문제입니다.
- 특정 mono 런타임 작동이 중단 됩니다.
- 일부 objective-c 예외와 충돌 합니다.
- 일부 프로세스 수명 초기 작동이 중단 됩니다.
- 일부 스택 오버플로가 발생 합니다.
- 에 나열 된 macOS 버전 하 **Info.plist** 현재 설치 된 macOS 버전 이거나 잘못 되었습니다. 보다 최신 버전입니다.

이러한 프로그램을 디버깅 답답할 수, 찾기도 필요한 정보를 어려울 수 있습니다. 방법이 도움이 될 수 있는 몇 가지는 다음과 같습니다.

- 에 나열 된 macOS 버전 확인 합니다 **Info.plist** 현재 컴퓨터에 설치 되어 있는 macOS의 버전과 사용 하는 것과 동일 합니다.
- Visual Studio를 Mac 응용 프로그램 출력에 대 한 확인 (**뷰** -> **패드** -> **응용 프로그램 출력**)에 대 한 스택 추적 또는 빨간색에서 출력 Cocoa는 출력을 설명할 수 있습니다.
- 명령줄에서 응용 프로그램을 실행 하 고 출력을 살펴 (에 **터미널** 앱)를 사용 하 여: 

     `MyApp.app/Contents/MacOS/MyApp` (여기서 `MyApp` 응용 프로그램의 이름입니다)
- 예를 들어 명령줄에서 명령에 "MONO_LOG_LEVEL"를 추가 하 여 출력을 높일 수 있습니다. 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- 네이티브 디버거를 연결할 수 없습니다 (`lldb`) 프로세스 참조 되 면 (유료 라이선스 필요) 더 이상 정보를 제공 합니다. 예를 들어, 다음을 수행 합니다.

    1. 입력 `lldb MyApp.app/Contents/MacOS/MyApp` 터미널에서.
    2. 입력 `run` 터미널에서.
    3. 입력 `c` 터미널에서.
    4. 디버깅을 완료 하는 경우 종료 합니다.
- 마지막 수단으로 호출 하기 전에 `NSApplication.Init` 에서 프로그램 `Main` 메서드 (또는 필요에 따라 다른 곳에서) 문제에 실행 중인 실행의 어떤 단계에서 추적 알려진된 위치에서 파일에 텍스트를 작성할 수 있습니다.

## <a name="known-issues"></a>알려진 문제

다음 섹션에서 알려진된 문제 및 해결 방법 설명.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>샌드박스가 적용 된 앱에서 디버거를 연결할 수 없습니다.

디버거는 기본적으로 샌드 박싱을 사용 하도록 설정 하면 수 없는 앱에 연결 되므로 TCP 통해 Xamarin.Mac 앱에 연결할 적절 한 권한을 사용 하지 않고 앱을 실행 하려고 하면 오류가 발생 하므로 *"연결할 수 없습니다. 디버거가"* 합니다. 

[![자격 편집](troubleshooting-images/debug01.png "자격 편집")](troubleshooting-images/debug01-large.png#lightbox)

합니다 **나가는 네트워크 연결을 허용 (클라이언트)** 권한 디버거에 대 한 필요를이 사용 하도록 설정 하면 일반적으로 디버깅 합니다. 없으면 디버깅할 수 없습니다, 이후 업데이트는 `CompileEntitlements` 에 대 한 대상 `msbuild` 디버그에 대 한 샌드박스를 작동 되는 앱만 빌드에 대 한 해당 권한을 자격을 자동으로 추가 합니다. 릴리스 빌드는 수정 되지 않은 자격 파일에서 지정 된 자격을 사용 해야 합니다.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 없습니다 데이터가 437 인코딩에 사용할 수 있습니다.
 
Xamarin.Mac 앱에서 타사 라이브러리를 포함 하는 경우 오류가 발생할 수 있습니다는 형태로 "System.NotSupportedException: 데이터를 없습니다 인코딩 437"컴파일 및 앱을 실행 하려고 할 때 사용할 수 있습니다. 예를 들어 라이브러리와 같은 `Ionic.Zip.ZipFile`를 작업 하는 동안이 예외를 throw 할 수 있습니다.

하려는 Xamarin.Mac 프로젝트에 대 한 옵션을 열어이 해결할 수 있습니다 **Mac 빌드** > **국제화** 확인 합니다 **서쪽** 국제화:

[![빌드 옵션 편집](troubleshooting-images/issue01.png "빌드 옵션 편집")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>(Mm5103) 컴파일하지 못했습니다.

이 오류는 새 버전의 Xcode 버전 이며 새 버전을 설치한 경우 하지만 더 실행 하는 아직 일반적으로 발생 됩니다. 새 버전의 Xcode 사용 하 여 컴파일 하기 전에 먼저 해당 버전을 한 번 이상 실행 해야 합니다.

Xcode의 새 버전을 실행 하면 처음으로 Xamarin.Mac에 필요한 몇 가지 명령줄 도구를 설치 합니다. 또한 Xcode 또는 Xamarin.Mac 버전을 업데이트 한 후 클린 빌드를 수행 해야 합니다.

이 문제를 해결할 수 없으면 하세요 [버그](#filing-a-bug)합니다.

### <a name="missing-entitlementsplist"></a>누락 된 entitlements.plist

최신 버전의 Mac 용 Visual Studio가에서 권한 부여 섹션을 제거 합니다 **Info.plist** 편집기 별개의 배치 **Entitlements.plist** (더 나은 플랫폼 간 지원에 대 한 편집기 xamarin.ios).

새 Visual Studio를 사용 하 여 Mac 설치를 만들면 새 Xamarin.Mac 앱 프로젝트를 **Entitlements.plist** 파일이 프로젝트 트리에 자동으로 추가 됩니다.

![자격을 선택](troubleshooting-images/entitlements01.png "자격을 선택")

두 번 클릭 합니다 **Entitlements.plist** 파일인 자격 편집기가 나타납니다.:

[![자격 편집](troubleshooting-images/entitlements02.png "자격 편집")](troubleshooting-images/entitlements02-large.png#lightbox)

수동으로 만들려고 해야 기존 Xamarin.Mac 프로젝트에 대 한 합니다 **Entitlements.plist** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 파일을 **Solution Pad** 선택 하 고 **추가**  >  **새 파일...** . 다음으로, 선택 **Xamarin.Mac** > **빈 속성 목록을**:

![새 속성 목록 추가](troubleshooting-images/entitlements03.png "새 속성 목록 추가")

입력 `Entitlements` 이름과 클릭 합니다 **새로 만들기** 단추입니다. 프로젝트가 이전에 자격 파일을 포함 하는 경우 새 파일을 만드는 대신 프로젝트에 추가할 묻는 메시지가 나타납니다.

[![파일 덮어쓰기 확인](troubleshooting-images/entitlements04.png "파일 덮어쓰기 확인")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>포럼에서 커뮤니티 지원

Xamarin 제품을 사용 하 여 개발자 커뮤니티 놀라운 이며 방문 대부분 우리의 [Xamarin.Mac 포럼](http://forums.xamarin.com/categories/mac) 경험 및 전문 지식을 공유 하 합니다. 또한 Xamarin 엔지니어 있도록 포럼을 방문 하는 주기적으로 합니다.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>버그 제출

여러분의 의견은 소중 합니다. Xamarin.Mac 사용 하 여 문제가 경우:

- [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 검색 
- GitHub 문제로 전환하기 전에 Xamarin 문제가 [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)에서 추적되었습니다. 여기서 일치하는 문제를 검색해 보세요.
- 일치하는 문제를 찾을 수 없는 경우 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 제출하세요.

GitHub 문제는 모두 공용입니다. 설명 또는 첨부 파일을 숨길 수 없습니다. 

다음 정보를 가능한 많이 포함하세요.                                                                                                                                          

- 문제를 재현하는 간단한 예제. 문제를 재현할 수 있다면 **매우 유용합니다**. 
- 크래시의 전체 스택 추적.
- 크래시 주변의 C# 코드. 
