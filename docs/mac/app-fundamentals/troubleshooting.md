---
title: 버그를 보고
description: 이 문서에서는 버그를 해결 하기 위한 방법에 설명 합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9b0d757c951f9244beb093a0a9b13ac1d069b507
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2018
---
# <a name="reporting-bugs"></a>버그를 보고

## <a name="overview"></a>개요

경우에 따라 원하는 방식으로 작동 하기 위한 API를 가져올 수 없어서 또는 버그를 해결 하는 프로젝트에서 작업 하는 동안 갇 우리는 모두 합니다. Xamarin에서 목표는 모바일 및 데스크톱 응용 프로그램 작성 성공 수 및 몇 가지 리소스를 제공 했습니다.

이러한 리소스를 일부의 단계를 신속 하 게 문제를 해결 하는 데 도움이 위해 수행할 수 있는 준비:

- 충돌을 보고 가능한 최선의 문제의 근본 원인을 확인 합니다.
 
     - "응용 프로그램이 충돌" 진단 하기가 어렵습니다. "응용 프로그램이이 호출에는 빈 배열을 반환 해야 하는 경우 충돌" 수정한 다음 하기가 훨씬 쉽습니다.

     - "내 NSTableDelegate 대 한 메서드 없음 기가 경우에 호출 됩니다." 보다 작은 것이 도움이 됩니다 "에서 실행 되도록 NSTable 받을 수 없는"

- 가능한 경우 문제를 보여 주는 작은 예제 프로그램을 제공 합니다. 더 많은 시간과 노력이 배나를 사용 하는 문제에 대 한 소스 코드의 페이지를 통해 조사 합니다.

- 기능 표시 문제가 발생할 수 있는 응용 프로그램에 대 한 변경 내용을 신속 하 게 식별할 수 문제의 원인을 파악 합니다. 최근에 문제를 일으킬 파트를 찾으려면 응용 프로그램의 섹션 아웃 트리밍 Xamarin.Mac의 버전을 업그레이드 한 경우 나 변경 내용 도입 문제 찾기에 빌드를 이전 테스트 하는 것은 매우 유용할 수 있습니다.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>앱이 출력 없이 충돌 하는 경우 수행할 작업

대부분의 경우에서 Mac 용 Visual Studio의 디버거 예외를 catch 하 고 응용 프로그램에서 충돌 하 고 근본 원인을 추적 하는 데 도움이 합니다. 그러나 출력을 거의 또는 전혀와 응용 프로그램을 도킹 스테이션에서 바운스 및 다음 종료 하려는 몇 가지 경우가 있습니다. 이러한 포함할 수 있습니다.

- 코드 서명 문제입니다.
- 특정 모노 런타임 충돌합니다.
- 일부 Objective c 예외와 충돌 합니다.
- 일부 프로세스 수명 초기 충돌 합니다.
- 일부 스택 오버플로 합니다.
- 에 나열 된 macOS 버전 프로그램 **Info.plist** 현재 설치 된 macOS 버전 이거나 잘못 되었습니다. 보다 최신 버전입니다.

이러한 프로그램을 디버깅 하기 어려울 수 있습니다, 그리고 찾기도 필요한 정보 어려울 수 있습니다. 다음은 도움이 될 수 있는 몇 가지 접근 방식입니다.

- 에 나열 된 macOS 버전을 확인 하는 **Info.plist** macOS 현재 컴퓨터에 설치 된 버전으로는 동일 합니다.
- Mac 응용 프로그램 출력에 대 한 Visual Studio를 확인 (**보기** -> **패드** -> **응용 프로그램 출력**)에 대 한 추적을 배치 또는에서 빨간색으로 출력 Cocoa 출력을 나타내는 수 있습니다.
- 명령줄에서 응용 프로그램을 실행 하 고 결과 확인 (에 **터미널** 응용 프로그램)를 사용 하 여: 

     `MyApp.app/Contents/MacOS/MyApp` (여기서 `MyApp` 응용 프로그램의 이름)
- 예를 들어 명령줄에서 명령에 "MONO_LOG_LEVEL"를 추가 하 여 출력을 높일 수 있습니다. 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- 네이티브 디버거를 연결할 수 있습니다 (`lldb`) 프로세스를 참조 하는 경우 제공 하는 더 이상 정보 (유료 라이선스 필요)에 있습니다. 예를 들어, 다음을 수행 합니다.

    1. 입력 `lldb MyApp.app/Contents/MacOS/MyApp` 터미널에서입니다.
    2. 입력 `run` 터미널에서입니다.
    3. 입력 `c` 터미널에서입니다.
    4. 디버깅 하는 완료 되 면 종료 합니다.
- 마지막 수단으로 호출 하기 전에 `NSApplication.Init` 에 프로그램 `Main` 메서드 (또는 필요에 따라 다른 위치에서)에서 시작의 어떤 단계 실행 하는 문제를 추적 하는 알려진된 위치에 파일에 텍스트를 작성할 수 있습니다.

## <a name="known-issues"></a>알려진 문제

다음 섹션에는 알려진된 문제 및 해결 방법 설명합니다.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>샌드 박싱된 응용 프로그램에 디버거를 연결할 수 없습니다.

기본적으로 샌드 박싱을 사용 하도록 설정 하면 해당 하지 않음을 응용 프로그램에 연결할 수, TCP 통해 Xamarin.Mac 앱에 디버거 연결 적절 한 권한을 사용 하지 않고 앱을 실행 하려고 하면 오류가 발생 하므로 *"연결할 수 없습니다. 디버거"*합니다. 

[![자격 편집](troubleshooting-images/debug01.png "자격 편집")](troubleshooting-images/debug01-large.png#lightbox)

**나가는 네트워크 연결 허용 (클라이언트)** 권한에 디버거에 대 한 필요한,이 중 하나를 사용 하도록 설정 하면 정상적으로 디버깅 합니다. 없이 디버깅할 수 없습니다, 이후 업데이트는 `CompileEntitlements` 에 대 한 대상 `msbuild` 가 디버그에 대 한 보안으로 보호 하는 앱만 빌드에 대해 해당 사용 권한을 자격에 자동으로 추가 합니다. 릴리스 빌드는 수정 되지 않은 자격 파일에 지정 된 자격을 사용 해야 합니다.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 데이터가 없는 ´ ü ± 437 인코딩
 
형식에서 오류가 발생할 수를 포함 하는 타사 라이브러리 Xamarin.Mac 응용 프로그램에서 "System.NotSupportedException: 데이터가 없는 ´ ü ± 437 인코딩" 컴파일하고 응용 프로그램을 실행 하려고 할 때입니다. 예를 들어 라이브러리와 같은 `Ionic.Zip.ZipFile`, 작업 중이 예외를 throw 할 수 있습니다.

Xamarin.Mac 프로젝트에 대 한 옵션을 열어이 해결할 수 있습니다 **Mac 빌드** > **국제화** 및 검사는 **서쪽** 국제화:

[![빌드 옵션 편집](troubleshooting-images/issue01.png "빌드 옵션 편집")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>(Mm5103) 컴파일하지 못했습니다.

새 버전의 Xcode 릴리스는 새 버전을 설치 하지만 더 실행 하는 것이 오류는 일반적으로 발생 됩니다. Xcode의 새 버전으로 컴파일하을 하기 전에 먼저 해당 버전을 한 번 이상 실행 해야 합니다.

처음으로 Xcode의 새 버전을 실행 하면 Xamarin.Mac에 필요한 몇 가지 명령줄 도구를 설치 합니다. 또한 Xcode 또는 Xamarin.Mac 버전을 업데이트 한 후 클린 빌드를 수행 해야 합니다.

이 문제를 해결할 수 없는 경우 다음 문의 [버그를 보고할](#filing-a-bug)합니다.

### <a name="missing-entitlementsplist"></a>누락 된 entitlements.plist

Mac 용 Visual Studio의 최신 버전에서 자격 섹션은 제거는 **Info.plist** 편집기와 별도의 배치 **Entitlements.plist** (더 나은 크로스 플랫폼 지원에 대 한 편집기 Xamarin.iOS)와.

새 Visual Studio와 함께 Mac 설치 하면 만들 때 새 Xamarin.Mac 앱 프로젝트는 **Entitlements.plist** 파일이 프로젝트 트리를 자동으로 추가 됩니다.

![자격을 선택 하면](troubleshooting-images/entitlements01.png "자격 선택")

두 번 클릭 하면는 **Entitlements.plist** 파일인 자격 편집기 표시 됩니다.

[![자격 편집](troubleshooting-images/entitlements02.png "자격 편집")](troubleshooting-images/entitlements02-large.png#lightbox)

수동으로 만들어야 할 Xamarin.Mac의 기존 프로젝트에는 **Entitlements.plist** 파일에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 여는 **솔루션 패드** 선택 하 고 **추가**  >  **새 파일...** . 다음으로, 선택 **Xamarin.Mac** > **빈 속성 목록을**:

![새 속성 목록을 추가할](troubleshooting-images/entitlements03.png "새 속성 목록 추가")

입력 `Entitlements` 클릭 이나 이름에는 **새로** 단추입니다. 프로젝트가 이전에 자격 파일을 포함 하는 경우 새 파일을 만드는 대신 프로젝트에 추가 하 라는 메시지가 표시 됩니다.

[![파일 덮어쓰기 확인](troubleshooting-images/entitlements04.png "파일 덮어쓰기 확인")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="contacting-support-business-or-enterprise-licenses"></a>(비즈니스 또는 엔터프라이즈 라이선스) 지원에 문의

비즈니스 또는 엔터프라이즈 라이선스가 있으면 모르는 경우 Xamarin 엔지니어를 통해 지원 티켓에서 직접 도움을 요청 하기에 적합 합니다. 참조 [xamarin.com/support](http://xamarin.com/support) 대 한 자세한 내용은 합니다.

## <a name="community-support-on-the-forums"></a>커뮤니티 지원 포럼에

Xamarin 제품을 사용 하는 개발자의 커뮤니티는 놀라운 일 이며 많은 방문 우리의 [Xamarin.Mac 포럼](http://forums.xamarin.com/categories/mac) 환경 및 해당 전문 지식을 공유 합니다. 또한 Xamarin 엔지니어는 주기적으로 수 있도록 포럼을 방문 합니다.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>버그 제출

여러분의 의견은 소중 합니다. Xamarin.Mac 문제가 있는지 확인 하는 경우:

- [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 검색 
- GitHub 문제로 전환하기 전에 Xamarin 문제가 [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)에서 추적되었습니다. 여기서 일치하는 문제를 검색해 보세요.
- 일치하는 문제를 찾을 수 없는 경우 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 제출하세요.

GitHub 문제는 모두 공용입니다. 설명 또는 첨부 파일을 숨길 수 없습니다. 

다음 정보를 가능한 많이 포함하세요.                                                                                                                                          

- 문제를 재현하는 간단한 예제. 문제를 재현할 수 있다면 **매우 유용합니다**. 
- 크래시의 전체 스택 추적.
- 크래시 주변의 C# 코드. 
