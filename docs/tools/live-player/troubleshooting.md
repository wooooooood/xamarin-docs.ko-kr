---
title: Xamarin Live Player를 문제 해결
description: 이 문서에서는 Xamarin Live Player 및 잠재적 해결 방법을 사용 하 여 알려진된 문제를 설명 합니다. 연결 문제, 구성 문제 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 08/08/2018
ms.openlocfilehash: ceb8964ac378957dcf5883bbbfff9e984b079294
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251227"
---
# <a name="troubleshooting-xamarin-live-player"></a>Xamarin Live Player를 문제 해결

![미리 보기 기능](~/media/shared/preview.png)

이 문서에서는 몇 가지 일반적인 문제에 설명 하 고 해결 하는 단계를 제공 합니다.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>바코드 스캔 (또는 코드 입력) 한 후 모바일 장치를 연결 하지 않습니다.

Xamarin Live Player를 실행 하는 모바일 장치는 IDE를 실행 하는 컴퓨터와 동일한 네트워크에 없는 경우 발생 합니다. 다음을 확인 합니다.

- 장치 및 컴퓨터 모두가 같은 Wi-fi 네트워크에 있는지 확인 합니다.
  - 그래도 컴퓨터는 유선된 네트워크에도 연결 되어, 유선된 연결을 분리 합니다.
- (예: 일부 회사 네트워크) 네트워크를 밀접 하 게 보호할 수 있습니다 Xamarin Live Player에 필요한 포트를 차단 합니다.
- Xamarin Live Player 앱을 닫고 다시 시작 합니다.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE에서 "배포 하는 동안 오류" 메시지가

**"IOException: 전송 연결에서 데이터를 읽을 수 없습니다: 비동기 소켓에 대 한 작업을 차단 하는"**

Xamarin Live Player를 실행 하는 모바일 장치가 Visual Studio를 실행 하는 컴퓨터와 동일한 네트워크에 없는 경우이 오류가 자주 발생 이전에 성공적으로 연결 된 장치에 연결 하는 경우에 자주 발생 합니다.

* 장치 및 컴퓨터 모두 같은 Wi-fi 네트워크에 있는지 확인 합니다.
* (예: 일부 회사 네트워크) 네트워크를 밀접 하 게 보호할 수 있습니다 Xamarin Live Player에 필요한 포트를 차단 합니다. 다음 포트에 Xamarin Live Player에 대 한 필요 합니다.
  * 37847-내부 네트워크 액세스 
  * 8090 – 외부 네트워크에 액세스

## <a name="manually-configure-device"></a>장치를 수동으로 구성

Wi-fi를 통해 장치에 연결 하지 수에 다음 단계를 사용 하 여 구성 파일을 통해 장치를 수동으로 구성 하려면 시작할 수 있습니다.

**1 단계: 구성 파일 열기**

응용 프로그램 데이터 폴더로 이동 합니다.

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

이 폴더에서 찾을 수 있습니다 **PlayerDeviceList.xml** 존재 하지 않는 경우 만들 해야 합니다.

**2 단계: IP 주소 가져오기**

Xamarin Live Player 앱으로 이동 **에 대 한 > 연결 테스트 > 연결 테스트 시작**합니다.

기록해 장치를 구성한 경우 나열 된 IP 주소는 IP 주소의 해야 합니다.

**3 단계: 연결 코드 가져오기**

Xamarin Live Player 탭 내의 **쌍** 또는 **쌍 다시**을 누릅니다 **수동으로 입력**합니다. 숫자 코드를 표시 됩니다, 구성 파일을 업데이트 해야 합니다.

**4 단계: GUID를 생성 합니다.**

로 이동: https://www.guidgenerator.com/online-guid-generator.aspx 새 guid를 생성 및 대문자로 켜져 있는지 확인 합니다.

**5 단계: 장치 구성**

엽니다는 **PlayerDeviceList.xml** 예: Visual Studio 또는 Visual Studio Code 편집기에서 설정 합니다. 이 파일에서 장치를 수동으로 구성 해야 합니다. 기본적으로 파일에 있어야 다음 빈 `Devices` XML 요소:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Android 장치를 추가 합니다.**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**페이지를 닫고 Visual Studio를 다시 엽니다.** 장치 목록에 표시 해야 합니다.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE에서 "형식 또는 네임 스페이스를 찾을 수 없습니다" 메시지

선택한 검사를 **시작 프로젝트** 일치 하는 장치 유형에 (예: Android) 구성 (예: 해당 장치 형식과 일치 **디버그** Android 용).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>플레이어에서 "형식의 생성자를 'InterpretedXamarin.Forms.Button' 찾을 수 없습니다" 메시지

일부 시스템 클래스 재정의할 수 없으며, 예를 들어:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout'에 'Main'에 대 한 정의가 없습니다."

Android 프로젝트에 대 한 AXML 파일에 정의 된 사용자 인터페이스를 사용 하 여이 오류를 발생 합니다.
Xamarin Live Player AXML 파일 현재 지원 되지 않습니다.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Xamarin.Forms 사용 하 여 올바르게 렌더링 android 도구 모음 및 탭

Xamarin.Forms Android 프로젝트 관련 레이아웃 파일의 이름에 "Toolbar.axml" 및 "Tabbar.axml"를 사용 해야 합니다. 기본 템플릿을 사용 하 여 이러한 이름을; 바꿔 렌더링 문제가 발생 합니다.

추가 문제를 보고 하십시오 [bugzilla](https://aka.ms/live-player-report-issue)합니다.

## <a name="related-links"></a>관련 링크

- [제한 사항](~/tools/live-player/limitations.md)
- [설정](~/tools/live-player/install.md)
