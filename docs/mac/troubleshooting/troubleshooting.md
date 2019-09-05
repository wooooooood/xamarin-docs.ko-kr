---
title: Xamarin.ios 문제 해결 팁
description: 이 문서에서는 Xamarin.ios 응용 프로그램을 개발할 때 발생 하는 문제를 해결 하는 방법을 설명 합니다. 또한 지원을 받는 방법에 대해 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 6bc2990ef82e1bccd4f9e530eb67265eeae528a9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292147"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin.ios 문제 해결 팁

## <a name="overview"></a>개요

때로는 API를 사용 하 여 원하는 방식으로 또는 버그를 해결할 수 없는 경우에도 프로젝트에서 작업 하는 동안 문제가 발생할 수 있습니다. Xamarin의 목표는 모바일 및 데스크톱 응용 프로그램을 작성 하는 데 도움이 되는 것 이며, 도움이 되는 몇 가지 리소스를 제공 했습니다.

이러한 리소스를 사용 하 여 문제를 신속 하 게 해결 하는 데 도움이 되는 준비 단계가 있습니다.

- 충돌 보고를 위해 가능한 한 가장 적절 한 문제의 근본 원인을 확인 합니다.

  - "내 응용 프로그램 충돌"은 진단 하기 어렵습니다. "이 호출로 빈 배열을 반환 하면 내 응용 프로그램의 작동이 중단 됩니다."는 수정 작업에 훨씬 더 쉽습니다.

  - "NSTable 된 작업을 수행할 수 없습니다."는 "내 NSTableDelegate에 어떠한 메서드도 호출 되지 않는 것 같습니다." 보다는 유용 하지 않습니다.

- 가능 하면 문제를 보여 주는 작은 예제 프로그램을 제공 합니다. 문제를 찾는 소스 코드 페이지를 살펴보면 더 많은 시간과 노력이 필요 합니다.

- 응용 프로그램에서 문제가 발생 하는 것으로 인 한 변경 내용을 알면 문제의 원인을 빠르게 좁힐 수 있습니다. 최신 버전의 Xamarin.ios를 최근에 업그레이드 한 경우 응용 프로그램의 섹션을 확장 하 여 문제의 원인이 되는 파트를 찾거나 이전 빌드를 테스트 하 여 문제가 발생 하는 변경 내용을 확인 하는 것이 매우 유용할 수 있습니다.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>출력 없이 앱이 충돌 하는 경우 수행할 작업

대부분의 경우 Mac용 Visual Studio 디버거는 응용 프로그램에서 예외 및 충돌을 포착 하 고 근본 원인을 추적 하는 데 도움을 줍니다. 그러나 일부 경우에는 응용 프로그램이 도킹을 통해 바운스 된 다음 출력을 거의 또는 전혀 사용 하지 않고 종료 됩니다. 여기에는 다음이 포함 될 수 있습니다.

- 코드 서명 문제.
- 특정 mono 런타임이 충돌 합니다.
- 일부 목표-c 예외 및 충돌.
- 일부 충돌은 프로세스 수명의 초기에 발생 합니다.
- 일부 스택 오버플로
- **Info.plist** 에 나열 된 macos 버전이 현재 설치 된 macos 버전 보다 최신 이거나 잘못 되었습니다.

이러한 프로그램을 디버깅 하는 데 필요한 정보를 찾기가 어려울 수 있기 때문에 어려울 수 있습니다. 다음은 도움이 될 수 있는 몇 가지 방법입니다.

- Info.plist에 나열 된 macOS 버전이 현재 컴퓨터에 설치 된 macOS 버전과 동일 해야 합니다 **.**
- 출력을 설명할 수 있는 cocoa에서 스택 추적 또는 출력에 대 한 Mac용 Visual Studio 응용 프로그램 출력 (**보기** -> **패드** -> **응용 프로그램 출력**)을 확인 합니다.
- 명령줄에서 응용 프로그램을 실행 하 고 다음을 사용 하 여 **터미널** 앱의 출력을 확인 합니다.

  `MyApp.app/Contents/MacOS/MyApp`(여기서 `MyApp` 는 응용 프로그램의 이름)
- 명령줄에서 명령에 "MONO_LOG_LEVEL"를 추가 하 여 출력을 늘릴 수 있습니다. 예를 들면 다음과 같습니다.

  `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- 프로세스에 네이티브 디버거 (`lldb`)를 연결 하 여 더 많은 정보를 제공 하는지 확인할 수 있습니다 (유료 라이선스 필요). 예를 들어, 다음을 수행 합니다.

  1. 터미널 `lldb MyApp.app/Contents/MacOS/MyApp` 에을 입력 합니다.
  2. 터미널 `run` 에을 입력 합니다.
  3. 터미널 `c` 에을 입력 합니다.
  4. 디버깅이 완료 되 면 종료 합니다.
- 마지막 수단으로, `NSApplication.Init` `Main` 메서드 (또는 필요한 경우 다른 위치)에서를 호출 하기 전에 실행 중인 시작 단계를 추적 하기 위해 알려진 위치의 파일에 텍스트를 쓸 수 있습니다.

## <a name="known-issues"></a>알려진 문제

다음 섹션에서는 알려진 문제 및 해결 방법에 대해 설명 합니다.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>샌드박스 앱에서 디버거에 연결할 수 없습니다.

디버거는 TCP를 통해 Xamarin.ios 앱에 연결 합니다. 즉, 기본적으로 샌드 박싱을 사용 하도록 설정 하면 앱에 연결할 수 없습니다. 따라서 적절 한 사용 권한을 설정 하지 않고 앱을 실행 하려고 하면 *"디버거에 연결할 수 없습니다." 오류가 발생 합니다.* .

[![자격 편집](troubleshooting-images/debug01.png "자격 편집")](troubleshooting-images/debug01-large.png#lightbox)

디버거를 사용 하도록 설정 하는 데 필요한 **송신 네트워크 연결 허용 (클라이언트)** 권한이 디버거를 사용 하도록 설정 하는 것이 일반적입니다. 이 없이 디버그할 수 없으므로 디버그 빌드 전용으로 샌드 박싱된 `CompileEntitlements` 앱에 `msbuild` 대 한 자격에 해당 권한을 자동으로 추가 하도록 대상이 업데이트 되었습니다. 릴리스 빌드에서는 자격 파일에 지정 된 자격을 수정 되지 않은 상태로 사용 해야 합니다.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>NotSupportedException: encoding 437에 사용할 수 있는 데이터가 없습니다.

Xamarin.ios 앱에 타사 라이브러리를 포함 하는 경우 다음과 같은 오류 메시지가 표시 될 수 있습니다. "NotSupportedException: 응용 프로그램을 컴파일하고 실행 하려고 할 때 "encoding 437"에 사용할 수 있는 데이터가 없습니다. 예를 들어와 `Ionic.Zip.ZipFile`같은 라이브러리는 작업 중에이 예외를 throw 할 수 있습니다.

이는 mac 프로젝트에 대 한 옵션을 열고, **Mac 빌드** > 로 이동**하 고,** **서 부** 국제화를 확인 하 여 해결할 수 있습니다.

[![빌드 옵션 편집](troubleshooting-images/issue01.png "빌드 옵션 편집")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>컴파일하지 못했습니다 (mm5103).

이 오류는 일반적으로 새 버전의 Xcode가 릴리스이 고 새 버전을 설치 했지만 아직 실행 하지 않은 경우에 발생 합니다. 새 버전의 Xcode를 사용 하 여 컴파일하려면 먼저이 버전을 한 번 이상 실행 해야 합니다.

Xcode의 새 버전을 처음으로 실행 하면 Xamarin.ios에 필요한 몇 가지 명령줄 도구가 설치 됩니다. 또한 Xcode 또는 Xamarin.ios 버전을 업데이트 한 후에는 정리 빌드를 수행 해야 합니다.

이 문제를 해결할 수 없는 경우 [버그를 파일에](#filing-a-bug)입력 하세요.

### <a name="missing-entitlementsplist"></a>Info.plist가 없습니다.

최신 버전의 Mac용 Visual Studio는 **info.plist** 편집기에서 자격 섹션을 제거 하 고 **info.plist** 편집기에 배치 했습니다 (xamarin.ios를 사용 하 여 플랫폼 간 지원 향상).

새 Mac용 Visual Studio 설치 하면 새 Xamarin.ios 앱 프로젝트를 만들 때 **info.plist** 파일이 프로젝트 트리에 자동으로 추가 됩니다.

![자격 선택](troubleshooting-images/entitlements01.png "자격 선택")

**Info.plist** 파일을 두 번 클릭 하면 자격 편집기가 표시 됩니다.

[![자격 편집](troubleshooting-images/entitlements02.png "자격 편집")](troubleshooting-images/entitlements02-large.png#lightbox)

기존 xamarin.ios 프로젝트의 경우 **Solution Pad** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 하 여 **info.plist** 파일을 수동으로 만들어야 합니다. 그런 다음, **xamarin.ios** > **빈 속성 목록**을 선택 합니다.

![새 속성 목록 추가](troubleshooting-images/entitlements03.png "새 속성 목록 추가")

이름 `Entitlements` 으로를 입력 하 고 **새로 만들기** 단추를 클릭 합니다. 이전에 프로젝트에 자격 파일이 포함 되어 있는 경우 새 파일을 만드는 대신 프로젝트에 추가 하 라는 메시지가 표시 됩니다.

[![파일 덮어쓰기 확인](troubleshooting-images/entitlements04.png "파일 덮어쓰기 확인")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>포럼의 커뮤니티 지원

Xamarin 제품을 사용 하는 개발자 커뮤니티는 놀라운 것 이며 많은 [경험을 통해](http://forums.xamarin.com/categories/mac) 경험과 전문 지식을 공유할 수 있습니다. 또한 Xamarin 엔지니어가 포럼을 주기적으로 방문 하 여 도움을 줍니다.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>버그 파일링

사용자 의견은 microsoft에 중요 합니다. Xamarin.ios에서 문제가 발견 되 면 다음을 수행 합니다.

- [문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues) 검색
- GitHub 문제로 전환하기 전에 Xamarin 문제가 [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi)에서 추적되었습니다. 여기서 일치하는 문제를 검색해 보세요.
- 일치하는 문제를 찾을 수 없는 경우 [GitHub 문제 리포지토리](https://github.com/xamarin/xamarin-macios/issues/new)에서 새 문제를 제출하세요.

GitHub 문제는 모두 공용입니다. 설명 또는 첨부 파일을 숨길 수 없습니다.

다음 정보를 가능한 많이 포함하세요.

- 문제를 재현하는 간단한 예제. 문제를 재현할 수 있다면 **매우 유용합니다**.
- 크래시의 전체 스택 추적.
- 크래시 주변의 C# 코드.
