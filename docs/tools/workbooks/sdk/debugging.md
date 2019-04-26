---
title: 통합 디버그
description: 이 문서에서는 Xamarin Workbooks 통합, 에이전트 쪽 및 클라이언트 쪽 Windows Mac.에 디버깅 하는 방법 설명
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 86d9c6af93e7f59eb0e819730e46324688df7566
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61293740"
---
# <a name="debugging-integrations"></a>통합 디버그

## <a name="debugging-agent-side-integrations"></a>에이전트 측 통합 디버깅

제공 하는 로깅 메서드를 사용 하 여 가장 잘 이루어집니다 에이전트 쪽 통합 디버깅 합니다 `Log` 클래스의 `Xamarin.Interactive.Logging`합니다. 참조 된 [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) 호출 방법에 대 한 합니다.

MacOS에서 로그 메시지는 모두 로그 뷰어 메뉴에 표시 (**창 > 로그 뷰어**) 및 클라이언트 로그에 로깅합니다. Windows, 메시지만 나타납니다 클라이언트 로그에는 로그 뷰어가 있습니다.

클라이언트 로그는 macOS 및 Windows의 다음 위치에서.

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

알아두어야 할 한 가지는 통합 일반적인 통해 로드할 때 `#r` 개발 중 메커니즘을 통합 어셈블리 픽업 됩니다로 _종속성_ 통합 문서의 절대 경로 이면이 사용 하 여 패키지 및 사용 되지 않습니다. 이 변경 내용이 적용 되지 전파 하는 데 아무 않은 통합을 다시 작성 하는 경우 발생할 수 있습니다.

## <a name="debugging-client-side-integrations"></a>클라이언트 쪽 통합 디버깅

클라이언트 쪽 통합 JavaScript로 작성 되 고이 웹 브라우저 화면에 로드 (참조를 [아키텍처](~/tools/workbooks/sdk/architecture.md) 설명서)을 디버깅 하는 가장 좋은 방법은 WebKit 개발자 도구를 사용 하 여 Mac에서 되었거나 F12 선택기를 사용 하 여 Windows에서 .

두 가지 도구를 사용 하면 JavaScript/TypeScript 소스 보기, 중단점을 설정, 콘솔 출력을 확인 하 고 검사 하 고 수정할 DOM

### <a name="mac"></a>Mac

Mac에서 Xamarin Workbooks에 대 한 개발자 도구를 사용 하려면 터미널에서 다음 명령을 실행 합니다.

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

고 Xamarin Workbooks를 다시 시작 합니다. 이렇게 하면 표시 됩니다 **검사할 요소** 에 오른쪽 클릭 상황에 맞는 메뉴를 사용 하 여 새 표시 **개발자** 창 통합 문서 환경 설정에서 사용할 수 있습니다. 이 옵션을 사용 하면 시작 시 열린 개발자 도구를 사용 하려는 경우 선택할 수 있습니다.

[![개발자 창](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

이 기본 설정도 다시 시작 전용-새 통합 문서에 적용 하기 위해에서 통합 문서 클라이언트를 다시 시작 해야 합니다. 상황에 맞는 메뉴 또는 기본 설정을 통해 개발자 도구를 활성화 하면 친숙 한 Safari UI 표시 됩니다.

[![Safari 개발 도구](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Safari 개발자 도구를 사용 하는 방법에 대 한 내용은 참조는 [WebKit 검사기 설명서][webkit-docs]합니다.

### <a name="windows"></a>Windows

Windows, IE 팀 제공 "F12 선택" 이라고 하는 도구 포함 된 Internet Explorer 인스턴스에 대해 원격 디버거입니다. 다음 위치에서 도구를 찾을 수 있습니다.

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

실행된 F12 선택 하는 목록에서 통합 문서 클라이언트 화면을 구동 하는 포함 된 인스턴스를 확인 해야 합니다. 선택 및 Internet Explorer에서 디버깅 도구 나타납니다 친숙 한 F12 클라이언트에 연결 합니다.

[![F12 도구](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
