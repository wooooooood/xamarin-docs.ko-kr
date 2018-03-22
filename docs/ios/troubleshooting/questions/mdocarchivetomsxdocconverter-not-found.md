---
title: MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest
ms.topic: article
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1e49c270d5836379d60f50ec72960ddc83bfbba4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest

> [!IMPORTANT]
> 최신 버전의 Xamarin에이 문제가 해결 되었습니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.


## <a name="error-message"></a>오류 메시지

이 오류에 나타날 수는 *Mac 서버 로그* Visual Studio에서:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

이 메시지에 2 별도 문제를 가지 있습니다.

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    이 오류는 무시 해도, 하지만 또한 잘못 되었습니다. 그 [제거 될 예정](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) 이후 릴리스에서 합니다.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    이 오류는 심각한 문제가 됩니다. 그러나로 인해는 [제한](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) 이 예외 스택 추적은 *불완전 한*합니다. Mac 서버 로그에 다음과 같이 불완전 한 스택 추적을 발견 하는 경우를 확인할 수 있습니다는 `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` 전체 스택 추적 찾을 Mac 빌드 호스트 파일에 있습니다.
