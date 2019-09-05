---
title: MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: d8c0c958a6aaad2603f6571a3531b7338d8cdab0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290999"
---
# <a name="mdocarchivetomsxdocconverterexe-not-found-rverbasecommandonrequest"></a>MDocArchiveToMsxDocConverter.exe에서 rver.BaseCommand.OnRequest를 찾을 수 없습니다.

> [!IMPORTANT]
> 이 문제는 최신 버전의 Xamarin에서 해결 되었습니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.


## <a name="error-message"></a>오류 메시지

이 오류는 Visual Studio의 *Mac 서버 로그* 에 나타날 수 있습니다.

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

이 메시지에는 두 가지 별도의 문제가 있습니다.

1. `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    이 오류는 무해 하지만 잘못 된 경우도 있습니다. 이후 릴리스에서 [제거 될 예정](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) 입니다.

2. `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    이 오류는 실제 문제입니다. 아쉽게도이 예외 스택 추적이 *불완전*한 [제한](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) 으로 인해 발생 합니다. Mac Server 로그에 이와 같이 불완전 한 스택 추적이 있는 경우 mac 빌드 호스트의 `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` 파일을 확인 하 여 전체 스택 추적을 찾을 수 있습니다.
