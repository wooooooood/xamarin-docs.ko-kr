---
title: 문제 해결 Xamarin Live Player
description: 이 문서에서는 Xamarin Live Player에 대 한 알려진 문제 및 잠재적 수정 사항에 대해 설명 합니다. 연결 문제, 구성 문제 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: conceptdev
ms.author: crdun
ms.date: 06/13/2019
ms.openlocfilehash: 04a377bad42ff680247759036327035d61757b42
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290173"
---
# <a name="troubleshooting-xamarin-live-player"></a>문제 해결 Xamarin Live Player

![미리 보기 기능](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player 미리 보기가 종료 되었습니다. 앱을 더 이상 사용할 수 없습니다. 아래 지침은 Visual Studio 2017 .에서 미리 보기를 계속 사용 하는 고객을 위해 제공 됩니다.

> [!TIP]
> Visual Studio 2019 또는 Mac용 Visual Studio의 [XAML 미리 보기](~/xamarin-forms/xaml/xaml-previewer/index.md) 를 사용 하 여 편집 하는 동안 화면 디자인을 볼 수 있습니다.

이 문서에서는 Live Player의 제한 사항 및이를 해결 하는 단계에 대 한 몇 가지 일반적인 문제에 대해 설명 합니다.

## <a name="limitations-of-xamarin-live-player"></a>Xamarin Live Player 제한 사항

### <a name="ide-requirements"></a>IDE 요구 사항

Live Player 미리 보기는 Visual Studio 2017 에서만 사용할 수 있습니다.

### <a name="device-requirements"></a>장치 요구 사항

Xamarin Live Player 앱은 다음 Android 장치를 지원 합니다.

- Android 4.2 이상.
- ARM-armeabi-v7a, ARM-arm64-v8a, ARM64-arm64-v8a, x86 또는 x86_64 processor.

### <a name="ios-limitations"></a>iOS 제한 사항

IOS에서 Live Player를 사용할 수 없습니다.

### <a name="xamarinforms-limitations"></a>Xamarin 폼 제한 사항

- 사용자 지정 렌더러는 지원 되지 않습니다.
- 효과는 지원 되지 않습니다.
- 사용자 지정 바인딩 가능 속성이 있는 사용자 지정 컨트롤은 지원 되지 않습니다.
- 포함 리소스는 지원 되지 않습니다 (즉, PCL에 이미지 또는 기타 리소스 포함).
- 타사 MVVM 프레임 워크는 지원 되지 않습니다 (ie. 프리즘, Mvvm Cross, Mvvm Light 등).

### <a name="other-project-type-limitations"></a>기타 프로젝트 형식 제한 사항

- 라이브 플레이어는 Android XML을 사용 하 여 사용자 인터페이스를 사용 하는 네이티브 Android 프로젝트용이 아닙니다.

### <a name="miscellaneous-limitations"></a>기타 제한 사항

- 리플렉션 지원이 제한적으로 지원 됩니다 (현재 SQLite 및 Json.NET와 같은 인기 있는 Nuget에 영향을 줌). 다른 Nuget 계속 지원 될 수 있습니다.
- 일부 시스템 클래스는 재정의할 수 없습니다. 예를 들어 서브 클래스를 구현할 수 없습니다.
- 프로 비전이 필요한 일부 플랫폼 기능은 Xamarin Live Player 앱에서 작동할 수 없습니다. 그러나 사진 갤러리 액세스와 같은 일반적인 작업을 위해 구성 되었습니다.
- 사용자 지정 대상 및 빌드 단계는 무시 됩니다. 예를 들어, Andy, Refit, AutoFac 및 AutoMapper와 같은 도구는 통합할 수 없습니다.
- F#프로젝트가 지원 되지 않습니다.
- 사용자 지정 제네릭 클래스 및 인터페이스가 포함 된 고급 시나리오는 지원 되지 않을 수 있습니다.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>바코드를 스캔 하거나 코드를 입력 한 후에는 모바일 장치가 연결 되지 않습니다.

Xamarin Live Player 실행 중인 모바일 장치가 IDE를 실행 하는 컴퓨터와 동일한 네트워크에 있지 않을 때 발생 합니다. 다음을 확인 하세요.

- 장치와 컴퓨터가 동일한 Wi-fi 네트워크에 있는지 확인 합니다.
  - 컴퓨터가 유선 네트워크에도 연결 되어 있으면 유선 연결을 분리 해 봅니다.
- 네트워크를 긴밀 하 게 보안 (예: 일부 회사 네트워크) 하 여 Xamarin Live Player에 필요한 포트를 차단할 수 있습니다.
- Xamarin Live Player 앱을 닫고 다시 시작 합니다.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE에서 "배포를 시도 하는 동안 오류 발생"

**"IOException: 전송 연결에서 데이터를 읽을 수 없습니다. 차단 되지 않는 소켓에 대 한 작업에서 "**

이 오류는 Xamarin Live Player를 실행 하는 모바일 장치가 Visual Studio를 실행 하는 컴퓨터와 동일한 네트워크에 있지 않은 경우에 종종 발생 합니다. 이는 이전에 성공적으로 페어링 된 장치에 연결할 때 주로 발생 합니다.

- 장치와 컴퓨터가 동일한 Wi-fi 네트워크에 있는지 확인 합니다.
- 네트워크를 긴밀 하 게 보안 (예: 일부 회사 네트워크) 하 여 Xamarin Live Player에 필요한 포트를 차단할 수 있습니다. Xamarin Live Player에는 다음 포트가 필요 합니다.
  - 37847 – 내부 네트워크 액세스 
  - 8090 – 외부 네트워크 액세스

## <a name="manually-configure-device"></a>수동으로 장치 구성

Wi-fi를 통해 장치에 연결할 수 없는 경우 다음 단계에 따라 구성 파일을 통해 장치를 수동으로 구성할 수 있습니다.

**1단계: 구성 파일 열기**

응용 프로그램 데이터 폴더에 대 한 헤드:

- Windows: **%userprofile%\AppData\Roaming**
- macOS: **~/Users/$USER/.config**

이 폴더에는 **PlayerDeviceList** 이 없는 경우 새로 만들어야 합니다.

**2단계: IP 주소 가져오기**

Xamarin Live Player 앱에서 **정보 > 연결 테스트로 이동 하 > 연결 테스트를 시작**합니다.

IP 주소를 기록해 둡니다. 장치를 구성할 때 표시 되는 IP 주소가 필요 합니다.

**3단계: 페어링 코드 가져오기**

Xamarin Live Player 내에서 **쌍** 또는 쌍을 **다시**탭 한 다음 enter 키를 **수동으로**누릅니다. 구성 파일을 업데이트 하는 데 필요한 숫자 코드가 표시 됩니다.

**4단계: GUID 생성**

다음 https://www.guidgenerator.com/online-guid-generator.aspx 으로 이동 하 여 새 guid를 생성 하 고 대문자가 on 인지 확인 합니다.

**5단계: 장치 구성**

Visual Studio 또는 Visual Studio Code와 같은 편집기에서 **PlayerDeviceList** 를 엽니다. 이 파일에서 장치를 수동으로 구성 해야 합니다. 기본적으로이 파일은 다음과 같은 빈 `Devices` XML 요소를 포함 해야 합니다.

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Android 장치 추가:**

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

**Visual Studio를 닫았다가 다시 엽니다.** 장치가 목록에 표시 됩니다.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE의 "형식 또는 네임 스페이스를 찾을 수 없습니다." 메시지

장치 유형과 일치 하는 **시작 프로젝트** 를 선택 했는지 확인 합니다 (예: Android)를 구성 하 고 해당 장치 유형 (예: Android 용 **디버그** ).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"InterpretedXamarin ' 형식의 생성자가 플레이어에서 메시지를 찾을 수 없습니다."

일부 시스템 클래스는 재정의할 수 없습니다. 예를 들면 다음과 같습니다.

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: ' Resource. Layout '에 ' Main '의 정의가 포함 되어 있지 않습니다.

이 오류는 AXML 파일에 정의 된 사용자 인터페이스를 사용 하는 Android 프로젝트에 대해 발생 합니다.
AXML 파일은 현재 Xamarin Live Player에서 지원 되지 않습니다.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android 도구 모음 및 탭이 Xamarin.ios를 사용 하 여 잘못 렌더링 됩니다.

Xamarin. Forms Android 프로젝트는 "Toolbar. axml" 및 "Tabbar. axml"을 사용 하 여 관련 레이아웃 파일의 이름을 사용 해야 합니다. 기본 템플릿에서는 이러한 이름을 사용 합니다. 이름을 바꾸면 렌더링 문제가 발생 합니다.

## <a name="related-links"></a>관련 링크

- [설정](~/tools/live-player/install.md)
