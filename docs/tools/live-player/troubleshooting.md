---
title: "문제 해결"
description: "Xamarin 라이브 Player 및 패키지를 수정 하는 방법의 알려진된 문제입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>문제 해결

![미리 보기 기능](~/media/shared/preview.png)

이 문서는 몇 가지 일반적인 문제에 설명 하 고 해결 단계를 제공 합니다.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>스캔 한 바코드 (또는 시작 중 코드) 한 후 모바일 장치를 연결 되지 않습니다.

Xamarin Player 라이브 실행 하는 모바일 장치는 IDE를 실행 하는 컴퓨터와 동일한 네트워크에 없을 때 발생 합니다. 다음을 확인 합니다.

- 장치 및 컴퓨터 모두 동일한 WiFi 네트워크에 있는지 확인 합니다.
  - 컴퓨터는 유선된 네트워크에도 연결 됩니다, 유선된 연결을 분리 하십시오.
- 네트워크 밀접 하 게 (예: 일부 기업 네트워크)를 보안 할 수 있습니다 Xamarin Player 라이브에 필요한 포트를 차단 합니다.
- Xamarin Player 라이브 응용 프로그램을 닫고 다시 시작 합니다.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE에서 "배포 하는 동안 오류" 메시지가

**"IOException: 전송 연결에서 데이터를 읽을 수 없습니다: 비동기 소켓에 대 한 작업이 차단"**

Xamarin Player 라이브 실행 하는 모바일 장치; IDE를 실행 하는 컴퓨터와 동일한 네트워크에 없는 경우이 오류가 자주 발생 이 오류 이전에 성공적으로 연결 된 장치에 연결할 때 자주 발생 합니다.

* 장치 및 컴퓨터 모두 동일한 WiFi 네트워크에 있는지 확인 합니다.
* 네트워크 밀접 하 게 (예: 일부 기업 네트워크)를 보안 할 수 있습니다 Xamarin Player 라이브에 필요한 포트를 차단 합니다. 다음 포트는 라이브 Xamarin Player 필요 합니다.
  * 37847-내부 네트워크 액세스 
  * 8090 – 외부 네트워크에 액세스

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE에서 "형식 또는 네임 스페이스를 찾을 수 없습니다" 메시지가

선택 된 검사는 **시작 프로젝트** (iOS 또는 Android) 장치 유형에 일치 하는 구성을 해당 장치 유형 (예: 철자와 일치. **디버그 | iPhone 시뮬레이터** iOS 용).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>플레이어에서 "형식의 생성자를 찾을 수 없습니다 ' InterpretedXamarin.Forms.Button'" 메시지

일부 시스템 클래스 재정의할 수 없으며, 예:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout' 'Main'에 대 한 정의 포함 하지 않습니다"

이 오류는 AXML 파일에 정의 된 사용자 인터페이스를 가진 Android 프로젝트에 발생 합니다.
Xamarin Player 라이브 AXML 파일 현재 지원 되지 않습니다.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Xamarin.Forms 사용 하 여 올바르게 렌더링 android 도구 모음 및 탭

Xamarin.Forms Android 프로젝트는 관련 된 레이아웃 파일의 이름에 대 한 "Toolbar.axml" 및 "Tabbar.axml"를 사용 해야 합니다. 이러한 이름은;을 사용 하 여 기본 서식 파일 이름을 바꾸지 하면 렌더링 문제.


추가 문제를 보고 하십시오 [: bugzilla](https://aka.ms/live-player-report-issue)합니다.


## <a name="related-links"></a>관련 링크

- [제한 사항](~/tools/live-player/limitations.md)
- [설정](~/tools/live-player/install.md)
