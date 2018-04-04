---
title: 문제 해결
description: Xamarin 라이브 Player 및 패키지를 수정 하는 방법의 알려진된 문제입니다.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: f9d5d0e4a2217d48577c60940fb027b3a77416d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>문제 해결

![미리 보기 기능](~/media/shared/preview.png)

이 문서는 몇 가지 일반적인 문제에 설명 하 고 해결 단계를 제공 합니다.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>스캔 한 바코드 (또는 시작 중 코드) 한 후 모바일 장치를 연결 되지 않습니다.

Xamarin Player 라이브 실행 하는 모바일 장치는 IDE를 실행 하는 컴퓨터와 동일한 네트워크에 없을 때 발생 합니다. 다음을 확인 합니다.

- 장치 및 컴퓨터 모두 동일한 Wi-fi 네트워크에 있는지 확인 합니다.
  - 컴퓨터는 유선된 네트워크에도 연결 됩니다, 유선된 연결을 분리 하십시오.
- 네트워크 밀접 하 게 (예: 일부 기업 네트워크)를 보안 할 수 있습니다 Xamarin Player 라이브에 필요한 포트를 차단 합니다.
- Xamarin Player 라이브 응용 프로그램을 닫고 다시 시작 합니다.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE에서 "배포 하는 동안 오류" 메시지가

**"IOException: 전송 연결에서 데이터를 읽을 수 없습니다: 비동기 소켓에 대 한 작업이 차단"**

Xamarin Player 라이브 실행 하는 모바일 장치; Visual Studio를 실행 하는 컴퓨터와 동일한 네트워크에 없는 경우이 오류가 자주 발생 이 오류 이전에 성공적으로 연결 된 장치에 연결할 때 자주 발생 합니다.

* 장치 및 컴퓨터 모두 동일한 Wi-fi 네트워크에 있는지 확인 합니다.
* 네트워크 밀접 하 게 (예: 일부 기업 네트워크)를 보안 할 수 있습니다 Xamarin Player 라이브에 필요한 포트를 차단 합니다. 다음 포트는 라이브 Xamarin Player 필요 합니다.
  * 37847-내부 네트워크 액세스 
  * 8090 – 외부 네트워크에 액세스

## <a name="manually-configure-device"></a>수동으로 장치를 구성 합니다.

메일을 통해 장치에 연결 하지 않는 수를 다음 단계를 구성 파일을 통해 장치를 수동으로 구성 시작할 수 있습니다.

**1 단계: 구성 파일 열기**

응용 프로그램 데이터 폴더에 헤드:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

이 폴더에 있습니다 **PlayerDeviceList.xml** 존재 하지 않는 경우 하나 만듭니다 해야 합니다.

**2 단계: IP 주소 가져오기**

Xamarin Player 라이브 앱에서로 이동 **에 대 한 > 연결 테스트 > 연결 테스트 시작**합니다.

기록해 IP 주소의 장치를 구성 하는 경우 나열 된 IP 주소가 필요 합니다.

**3 단계: 연결 코드 가져오기**

Xamarin Player 라이브 tap 내부 **쌍** 또는 **쌍 다시**, 키를 누릅니다 **수동으로 입력**합니다. 한 숫자 코드는 표시, 구성 파일을 업데이트 해야 합니다.

**4 단계: GUID를 생성 합니다.**

로 이동: https://www.guidgenerator.com/online-guid-generator.aspx 및 새 guid를 생성 하 고 대문자로 켜져 있는지 확인 합니다.


**5 단계: 장치를 구성 합니다.**

열립니다는 **PlayerDeviceList.xml** 같은 Visual Studio 또는 Visual Studio Code 편집기의 합니다. 이 파일에서 장치를 수동으로 구성 해야 합니다. 기본적으로 파일에는 다음과 같은 빈 포함 되어야 `Devices` XML 요소:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**IOS 장치를 추가 합니다.**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
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

**페이지를 닫고 Visual Studio를 다시 엽니다.** 장치 목록에 나타나야 합니다.


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
