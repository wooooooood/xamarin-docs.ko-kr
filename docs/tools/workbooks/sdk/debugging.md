---
title: 통합 디버깅
description: 이 문서에서는 Windows 및 Mac에서 에이전트 쪽 및 클라이언트 쪽 Xamarin Workbooks 통합을 디버그 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: conceptdev
ms.author: crdun
ms.date: 06/19/2018
ms.openlocfilehash: fbb5673a70328ad6edde78af1b35d2801fe65ca8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283924"
---
# <a name="debugging-integrations"></a>통합 디버깅

## <a name="debugging-agent-side-integrations"></a>에이전트 쪽 통합 디버깅

`Log` 에서`Xamarin.Interactive.Logging`클래스가 제공 하는 로깅 메서드를 사용 하 여 에이전트 쪽 통합 디버깅을 수행 하는 것이 가장 좋습니다.

MacOS에서 로그 메시지는 로그 뷰어 메뉴 (**창 > 로그 뷰어**)와 클라이언트 로그 모두에 표시 됩니다. Windows에서는 로그 뷰어가 없으므로 메시지는 클라이언트 로그에만 표시 됩니다.

클라이언트 로그는 macOS 및 Windows의 다음 위치에 있습니다.

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

고려해 야 할 한 가지 사항은 개발 중에 일반적인 `#r` 메커니즘을 통해 통합을 로드할 때 통합 어셈블리가 통합 문서에 대 한 _종속성_ 으로 선택 되 고 절대 경로를 사용 하지 않는 경우 패키지에 포함 된다는 것입니다. 이로 인해 변경 내용이 전파 되지 않는 것 처럼 보일 수 있습니다.

## <a name="debugging-client-side-integrations"></a>클라이언트 쪽 통합 디버깅

클라이언트 쪽 통합은 JavaScript로 작성 되 고 웹 브라우저 화면에 로드 됩니다 ( [아키텍처](~/tools/workbooks/sdk/architecture.md) 설명서 참조) .이를 디버깅 하는 가장 좋은 방법은 Mac에서 WebKit 개발자 도구를 사용 하거나 Windows에서 F12 선택기를 사용 하는 것입니다.

두 도구 집합 모두 JavaScript/TypeScript 소스를 보고, 중단점을 설정 하 고, 콘솔 출력을 보고, DOM을 검사 및 수정할 수 있습니다.

### <a name="mac"></a>Mac

Mac에서 Xamarin Workbooks 용 개발자 도구를 사용 하도록 설정 하려면 터미널에서 다음 명령을 실행 합니다.

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

그런 다음 Xamarin Workbooks를 다시 시작 합니다. 이렇게 하면 오른쪽 클릭 상황에 맞는 메뉴에 **검사 요소가** 표시 되 고, 통합 문서 기본 설정에서 새 **개발자** 창이 제공 됩니다. 이 옵션을 사용 하면 시작 시 개발자 도구를 열 것인지 여부를 선택할 수 있습니다.

[![개발자 창](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

이 기본 설정도 다시 시작 전용입니다. 새 통합 문서에 적용 되려면 통합 문서 클라이언트를 다시 시작 해야 합니다. 상황에 맞는 메뉴 또는 기본 설정을 통해 개발자 도구를 활성화 하면 친숙 한 Safari UI가 표시 됩니다.

[![Safari 개발자 도구](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari 개발자 도구를 사용 하는 방법에 대 한 자세한 내용은 [WebKit inspector 설명서][webkit-docs]를 참조 하세요.

### <a name="windows"></a>Windows

Windows에서 IE 팀은 포함 된 Internet Explorer 인스턴스의 원격 디버거 인 "F12 선택기" 라는 도구를 제공 합니다. 다음 위치에서 도구를 찾을 수 있습니다.

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

F12 선택기를 실행 하 고 목록에서 통합 문서 클라이언트 화면을 구동 하는 포함 된 인스턴스가 표시 되어야 합니다. 이를 선택 하면 클라이언트에 연결 된 Internet Explorer의 친숙 한 F12 디버깅 도구가 표시 됩니다.

[![F12 도구](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
