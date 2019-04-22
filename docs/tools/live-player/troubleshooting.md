---
title: Xamarin Live Player를 문제 해결
description: 이 문서에서는 Xamarin Live Player 및 잠재적 해결 방법을 사용 하 여 알려진된 문제를 설명 합니다. 연결 문제, 구성 문제 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: 5eb0dcead230e0bb2e7d99241e5d8e5a4115f838
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855265"
---
# <a name="troubleshooting-xamarin-live-player"></a>Xamarin Live Player를 문제 해결

![미리 보기 기능](~/media/shared/preview.png)

> [!NOTE]
> 실시간 플레이어 미리 보기에만 Visual Studio 2017에서 제공 됩니다.

이 문서에서는 Live Player 및 단계를 사용 하 여 몇 가지 일반적인 문제를 수정 하 여 제한인을 설명 합니다.

## <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player의 제한 사항

### <a name="ide-requirements"></a>IDE 요구 사항

Live Player 미리 보기에만 Visual Studio 2017에서 제공 됩니다.

### <a name="device-requirements"></a>장치 요구 사항

Xamarin Live Player 앱에서는 다음 Android 장치를 지원합니다.

- Android 4.2 이상입니다.
- ARM-v7a, v8a ARM, ARM64-v8a x86 또는 x86_64 프로세서.

### <a name="ios-limitations"></a>iOS의 제한 사항

Live Player iOS에 대 한 제공 되지 않습니다.

### <a name="xamarinforms-limitations"></a>Xamarin.Forms 제한 사항

- 사용자 지정 렌더러 지원 되지 않습니다.
- 작업이 지원 되지 않습니다.
- 사용자 지정 바인딩 가능한 속성을 사용 하 여 사용자 지정 컨트롤을 사용 하는 것이 없습니다.
- 포함 된 리소스는 지원 되지 않습니다 (ie. PCL에 이미지 또는 기타 리소스를 포함).
- 타사 MVVM 프레임 워크 (즉 지원 되지 않습니다. Prism, Mvvm 간, Mvvm Light 등).

### <a name="other-project-type-limitations"></a>다른 프로젝트 형식 제한 사항

- (Android XML을 사용 하 여 사용자 인터페이스)는 네이티브 Android 프로젝트에 대 한 live Player는 사용 하는 것이 없습니다.

### <a name="miscellaneous-limitations"></a>기타 제한 사항

- 리플렉션에 대 한 지원이 제한 됩니다 (현재 SQLite Json.NET 등 몇 가지 인기 있는 Nuget을 줌). 다른 Nuget 계속 지원할 수 있습니다.
- 일부 시스템 클래스는 재정의할 수 없습니다 (예를 들어 하위 클래스를 구현할 수 없습니다).
- (그러나 구성 된 사진 갤러리 액세스과 같은 일반적인 작업에 대 한) Xamarin Live Player 앱에서 프로 비전 해야 하는 일부 플랫폼 기능 작업 수 없습니다.
- 사용자 지정 대상 및 빌드 단계는 무시 됩니다. 예를 들어 Fody, Refit, AutoFac을 및 AutoMapper와 같은 도구를 통합할 수 없습니다.
- F#프로젝트가 지원 되지 않습니다.
- 사용자 지정 제네릭 클래스 및 인터페이스를 사용 하 여 고급 시나리오를 지원할 수 있습니다.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>바코드 스캔 (또는 코드 입력) 한 후 모바일 장치를 연결 하지 않습니다.

Xamarin Live Player를 실행 하는 모바일 장치는 IDE를 실행 하는 컴퓨터와 동일한 네트워크에 없는 경우 발생 합니다. 다음을 확인 합니다.

- 장치 및 컴퓨터 모두가 같은 Wi-fi 네트워크에 있는지 확인 합니다.
  - 그래도 컴퓨터는 유선된 네트워크에도 연결 되어, 유선된 연결을 분리 합니다.
- (예: 일부 회사 네트워크) 네트워크를 밀접 하 게 보호할 수 있습니다 Xamarin Live Player에 필요한 포트를 차단 합니다.
- Xamarin Live Player 앱을 닫고 다시 시작 합니다.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE에서 "배포 하는 동안 오류" 메시지가

**"IOException: 전송 연결에서 데이터를 읽을 수 없습니다. 비블로킹 소켓에 대 한 작업은 차단 "**

Xamarin Live Player를 실행 하는 모바일 장치가 Visual Studio를 실행 하는 컴퓨터와 동일한 네트워크에 없는 경우이 오류가 자주 발생 이전에 성공적으로 연결 된 장치에 연결 하는 경우에 자주 발생 합니다.

* 장치 및 컴퓨터 모두 같은 Wi-fi 네트워크에 있는지 확인 합니다.
* (예: 일부 회사 네트워크) 네트워크를 밀접 하 게 보호할 수 있습니다 Xamarin Live Player에 필요한 포트를 차단 합니다. 다음 포트에 Xamarin Live Player에 대 한 필요 합니다.
  * 37847-내부 네트워크 액세스 
  * 8090 – 외부 네트워크에 액세스

## <a name="manually-configure-device"></a>장치를 수동으로 구성

Wi-fi를 통해 장치에 연결 하지 수에 다음 단계를 사용 하 여 구성 파일을 통해 장치를 수동으로 구성 하려면 시작할 수 있습니다.

**1단계: 열린 구성 파일**

응용 프로그램 데이터 폴더로 이동 합니다.

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

이 폴더에서 찾을 수 있습니다 **PlayerDeviceList.xml** 존재 하지 않는 경우 만들 해야 합니다.

**2단계: IP 주소 가져오기**

Xamarin Live Player 앱으로 이동 **에 대 한 > 연결 테스트 > 연결 테스트 시작**합니다.

기록해 장치를 구성한 경우 나열 된 IP 주소는 IP 주소의 해야 합니다.

**3단계: 연결 코드 가져오기**

Xamarin Live Player 탭 내의 **쌍** 또는 **쌍 다시**을 누릅니다 **수동으로 입력**합니다. 숫자 코드를 표시 됩니다, 구성 파일을 업데이트 해야 합니다.

**4단계: GUID를 생성 합니다.**

로 이동: https://www.guidgenerator.com/online-guid-generator.aspx 새 guid를 생성 및 대문자로 켜져 있는지 확인 합니다.

**5단계: 장치 구성**

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

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: "Resource.Layout' 'Main'에 대 한 정의"

Android 프로젝트에 대 한 AXML 파일에 정의 된 사용자 인터페이스를 사용 하 여이 오류를 발생 합니다.
Xamarin Live Player AXML 파일 현재 지원 되지 않습니다.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Xamarin.Forms 사용 하 여 올바르게 렌더링 android 도구 모음 및 탭

Xamarin.Forms Android 프로젝트 관련 레이아웃 파일의 이름에 "Toolbar.axml" 및 "Tabbar.axml"를 사용 해야 합니다. 기본 템플릿을 사용 하 여 이러한 이름을; 바꿔 렌더링 문제가 발생 합니다.

## <a name="related-links"></a>관련 링크

- [설정](~/tools/live-player/install.md)
- [Live Player를 사용 하는 샘플](https://developer.xamarin.com/samples/xamarin-live-player/all/)