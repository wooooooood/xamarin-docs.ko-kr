---
title: "디버깅 통합"
ms.topic: article
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a0873c6b902e29174da5e27a09e8f580d6d69eb7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="debugging-integrations"></a>디버깅 통합

## <a name="debugging-agent-side-integrations"></a>에이전트 측 통합 디버깅

제공 하는 로깅 메서드를 사용 하 여 에이전트 측 통합 디버깅을 수행할 가장는 `Log` 클래스 `Xamarin.Interactive.Logging`합니다. 참조는 [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) 호출 하는 방법에 대 한 합니다.

MacOS 등에서 로그 메시지가 모두 로그 뷰어 메뉴에 나타나는 (**창 > 로그 뷰어**)와 클라이언트 로그 합니다. Windows에서는 메시지에에서만 나타납니다 클라이언트 로그 그대로 로그 뷰어가 있습니다.

클라이언트 로그는 macOS 및 창에 다음 위치에서:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

한 알아야 할 점은 일반적인를 통해 통합을 로드할 때 `#r` 개발 하는 동안 메커니즘을 통합 어셈블리 선택 않을 것으로 _종속성_ 통합 문서의 경우 절대 경로와 패키지 사용 되지 않습니다. 이 인해 변경 nothing에서 통합을 다시 작성 하는 경우에 따라 전파 하지으로 나타날 수 있습니다.

## <a name="debugging-client-side-integrations"></a>클라이언트 쪽 통합 디버깅

클라이언트 쪽 통합 JavaScript로 작성 하 여이 웹 브라우저 화면에 로드 (참조는 [아키텍처](~/tools/workbooks/sdk/architecture.md) 설명서), 디버깅 하는 가장 좋은 방법은 WebKit 개발자 도구를 사용 하 여 Mac에서 또는 Windows에서 F12 선택을 사용 하는 .

두 가지 도구 허용 JavaScript/TypeScript 소스 보기, 중단점 설정, 콘솔 출력을 볼 및 검사 하 고 DOM 수정

### <a name="mac"></a>Mac

Mac에서 Xamarin 통합 문서에 대 한 개발자 도구를 사용 하려면 터미널에 다음 명령을 실행 합니다.

```shell
defaults write com.xamarin.Inspector WebKitDeveloperExtras -bool true
```

및 다음 Xamarin 통합 문서를 다시 시작 합니다. 이렇게 하면 표시 되어야 **검사 요소** 에 나타나고 오른쪽 클릭 상황에 맞는 메뉴를 사용 하 여 새 **개발자** 창 통합 문서 기본 설정에서 사용할 수 있습니다. 이 옵션을 사용 하면 개발자 도구 시작 시 열 여부를 선택할 수 있습니다.

[![개발자 창](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

이 기본 설정도 다시 시작 전용-클라이언트를 다시 시작 된 통합 문서에서 새 통합 문서에 적용 하기 위해 필요 합니다. 상황에 맞는 메뉴 또는 기본 설정을 통해 개발자 도구를 활성화 하면 친숙 한 Safari UI 표시 됩니다.

[![Safari 개발 도구](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari 개발자 도구를 사용 하는 방법에 대 한 정보를 참조 하십시오.는 [WebKit 검사기 설명서][webkit-docs]합니다.

### <a name="windows"></a>Windows

Windows에서는 IE 팀 "F12 선택" 라고 하는 도구를 제공 즉 포함 된 Internet Explorer의 인스턴스에 대 한 원격 디버거 합니다. 다음 위치에 도구를 찾을 수 있습니다.

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

실행된 F12 선택 하 고 목록에서 통합 문서 클라이언트 화면을 구동 하는 포함된 인스턴스가 표시 됩니다. 클라이언트에 연결, 및 Internet Explorer에서 디버깅 도구, 나타날 친숙 한 F12 선택:

[![F12 도구](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector