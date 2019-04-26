---
title: MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 0746174857f66843ef9a09429b6286f2efca90d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61420539"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe에서 rver.BaseCommand.OnRequest를 찾을 수 없습니다.

> [!IMPORTANT]
> Xamarin의 최신 버전에서이 문제가 해결 되었습니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.


## <a name="error-message"></a>오류 메시지

이 오류가 나타날 수는 *Mac 서버 로그* Visual Studio에서:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

이 메시지는 별도 문제가 2:

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    이 오류는 문제가 있지만 잘못 해석 됩니다. 것 [제거할](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) 릴리스에서 합니다.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    이 오류는 실제 문제. 아쉽게도로 인해를 [제한이](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) 이 예외 스택 추적 *불완전 한*합니다. Mac 서버 로그에 다음과 같이 완료 되지 않은 스택 추적을 보면 확인할 수 있습니다는 `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` 전체 스택 추적을 찾으려면 Mac 빌드 호스트에는 파일입니다.
