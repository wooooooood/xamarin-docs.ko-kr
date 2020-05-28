---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
robots: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b67594e2675c774512f3bf64f2e91ef10dbff444
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134214"
---
# <a name="xamarin-live-reload-preview"></a>Xamarin 라이브 다시 로드 (미리 보기)

> [!NOTE]
> Xamarin Live 다시 로드의 미리 보기가 종료 되었으며, 모든 사용자에 게 피드백과 의견을 보내 주셔서 감사 합니다. 
>
> 앱이 실행 중일 때 XAML을 편집 하려면 [에 대해 Xamarin.Forms Xaml 핫 다시 로드 ](~/xamarin-forms/xaml/hot-reload.md)를 사용 합니다.
>

Xamarin Live Reload를 사용 하면 **XAML을 변경 하 고 다른 컴파일 및 배포를 요구 하지 않고 라이브 상태를 볼**수 있습니다. XAML에 대 한 변경 내용은 저장 시 배포 대상에 다시 배포 됩니다.

## <a name="requirements"></a>요구 사항

* .NET 워크 로드를 사용 하는 **모바일 개발과** 함께 [Visual Studio 2017 버전 15.7 이상](https://visualstudio.microsoft.com/vs/)
* [ Xamarin.Forms 3.0.0 이상](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="getting-started"></a>시작하기
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Visual Studio Marketplace에서 Xamarin Live 다시 로드를 설치 합니다.

Xamarin Live Reload는 Visual Studio Marketplace를 통해 배포 됩니다. 확장을 설치 하려면 Visual Studio Marketplace 웹 사이트의 [Xamarin Live Reload 페이지](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) 를 방문 하 여 **다운로드**를 클릭 합니다.

다운로드 된 .vsix를 열고 **설치**를 클릭 합니다.

![Visual Studio 설치 관리자 Xamarin 라이브 다시 로드 확인](images/LiveReloadVSIXInstall.png)

또는 Visual Studio 내의 **확장 및 업데이트** 대화 상자에 있는 **온라인** 탭에서 검색할 수 있습니다.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. 라이브 다시 로드를 사용 하도록 앱 구성

기존 모바일 앱에 라이브 다시 로드를 추가 하는 작업은 다음 세 단계로 수행할 수 있습니다.

1. 모든 프로젝트가 [ Xamarin.Forms 3.0.0 이상을](https://www.nuget.org/packages/Xamarin.Forms/) 사용 하도록 업데이트 되었는지 확인 합니다.

2. **LiveReload** NuGet 패키지를 추가 합니다.

    a. **.NET Standard** – .NET Standard 2.0 라이브러리에 **LiveReload** NuGet을 설치 합니다. 이를 플랫폼 프로젝트에 설치할 필요는 없습니다. **패키지 원본이** **모두**로 설정 되어 있는지 확인 합니다.
    
    b. **공유 프로젝트** – **LiveReload** NuGet을 모든 플랫폼 프로젝트에 설치 합니다 (예: ANDROID, iOS, UWP 등). **패키지 원본이** **모두**로 설정 되어 있는지 확인 합니다.

    [![NuGet 패키지 관리자를 사용 하 여 Xamarin Live Reload NuGet 추가](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. `LiveReload.Init();`다음 코드 조각과 같이 클래스의 생성자에를 추가 합니다 `Application` .

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. 라이브 다시 로드 시작

응용 프로그램을 컴파일 및 배포 합니다. 앱이 배포 되 면 XAML 파일을 열고 일부를 변경한 후 파일을 저장 합니다. 변경 내용은 배포 대상에 다시 배포 됩니다.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

라이브 다시 로드는 XAML 파일에 대 한 변경 내용으로 작동 합니다. C #을 변경 하거나 NuGet 패키지를 추가/제거 하려면 새 빌드 및 배포를 적용 해야 합니다.

## <a name="frequently-asked-questions"></a>질문과 대답 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Mac용 Visual Studio에서 Xamarin Live 재로드를 사용할 수 있나요? 

아니요, Xamarin Live Reload의 미리 보기 릴리스는 Visual Studio 2017 에서만 사용할 수 있습니다.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>프리즘와 같은 모든 라이브러리에서 작동 하나요? 

앱이 컴파일되면 라이브 다시 로드는 프리즘와 같은 모든 라이브러리 및 Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity 및 기타 제어 공급 업체와 같은 타사 컨트롤 라이브러리와 함께 작동 합니다.

### <a name="what-changes-does-live-reload-redeploy"></a>라이브 다시 로드는 어떤 변경 내용이 배포 됩니까? 

라이브 다시 로드는 XAML 또는 CSS에 적용 된 변경 내용만 적용 합니다. C # 파일을 변경 하는 경우에는 다시 컴파일해야 합니다. 

### <a name="what-platforms-are-supported"></a>지원되는 플랫폼은 무엇인가요? 

라이브 다시 로드는 Xamarin.Forms Android, iOS 및 UWP를 비롯 하 여에서 지 원하는 모든 플랫폼에서 작동 합니다.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>이 작업은 에뮬레이터, 시뮬레이터 및 물리적 장치에서 작동 하나요? 

예, 라이브 다시 로드는 Android 에뮬레이터, iOS 시뮬레이터 및 물리적 장치를 비롯 한 모든 유효한 배포 대상과 함께 작동 합니다. 장치에 배포 하려면 장치와 컴퓨터가 동일한 Wi-fi 네트워크에 있어야 합니다.

### <a name="does-this-work-with-corporate-networks"></a>회사 네트워크에서 작동 하나요?

Android 에뮬레이터 또는 iOS 시뮬레이터에 디버깅 하는 경우 라이브 다시 로드는 localhost를 사용 하 여 통신 합니다. 장치에 배포 하려는 경우 장치와 컴퓨터가 동일한 Wi-fi 네트워크에 있어야 합니다. 이것이 가능 하지 않은 시나리오에서는 네트워크 연결 설정에 관계 없이 실시간 다시 로드를 수행할 수 있도록 [사용자 고유의 Live reload 서버를 구성할](#live-reload-server)수 있습니다.

### <a name="does-it-require-debugging-the-app"></a>앱을 디버깅 해야 하나요? 

아니요. 실제로 모든 수의 장치 또는 시뮬레이터/에뮬레이터에서 지원 되는 모든 응용 프로그램 목표 (Android, iOS 및 UWP)를 시작 하 고 모든 업데이트를 한 번에 볼 수 있습니다. 

## <a name="limitations"></a>제한 사항

* XAML의 다시 로드만 지원 됩니다.
* MVVM를 사용 하지 않는 한 다시 배포 간에 UI 상태가 유지 되지 않을 수 있습니다.

## <a name="known-issues"></a>알려진 문제

* Visual Studio 에서만 지원 됩니다.
* **링크를 연결 하지** 않거나 **프레임 워크 Sdk만 링크** 하지 않도록 설정 해야 합니다. 
* 앱 전체 리소스 (예: **app.xaml** 또는 공유 리소스 사전)를 다시 로드 하면 앱 탐색이 다시 설정 됩니다. 
* 현재 ContentView를 다시 로드 하려면 포함 하는 페이지를 다시 로드 해야 합니다.
* AutomationId를 포함 하는 요소는 다시 로드 오류가 발생할 수 있습니다.
* UWP를 디버깅할 때 XAML을 편집 하면 런타임 충돌이 발생할 수 있습니다. 해결 방법: **디버깅 시작 (F5)** 대신 **디버깅 하지 않고 시작 (Ctrl + F5)** 을 사용 합니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-codes"></a>오류 코드

* **XLR001**: *현재 프로젝트에서 ' LiveReload ' NuGet 패키지 버전 ' [version] '을 참조 하지만 Xamarin Live Reload 확장에 버전 ' [version] '이 (가) 필요 합니다.*

  라이브 다시 로드 기능의 빠른 반복 및 발전을 허용 하려면 NuGet 패키지와 Visual Studio 확장이 정확히 일치 해야 합니다. NuGet 패키지를 설치한 확장의 동일한 버전으로 업데이트 합니다.

* **XLR002**: *라이브 다시 로드는 명령줄에서 빌드할 때 적어도 ' MqttHostname ' 속성이 필요 합니다. 또는 ' EnableLiveReload '을 ' f a l s e '로 설정 하 여 기능을 사용 하지 않도록 설정 합니다.*

  라이브 다시 로드에 필요한 속성은 명령줄에서 빌드하는 경우 (또는 연속 통합에서) 사용할 수 없으므로 명시적으로 제공 해야 합니다. 

* **XLR003**: *라이브 다시 로드 NuGet 패키지를 사용 하려면 Xamarin Live reload Visual Studio 확장을 설치 해야 합니다.*

  라이브 다시 로드 NuGet 패키지를 참조 하는 프로젝트를 빌드하려고 했지만 시각적 확장이 설치 되어 있지 않습니다.  

* *어셈블리를 로드 하는 동안 예외가 발생 했습니다. System.io.filenotfoundexception: ' Xamarin.ios, Version = 0.3.27.0, Culture = 중립, PublicKeyToken = ' 어셈블리를 로드할 수 없습니다.*

  호스트 프로젝트는 대신를 사용 해야 합니다. `PackageReference``packages.config`

### <a name="app-doesnt-connect"></a>앱이 연결 되지 않음

응용 프로그램을 빌드할 때 **도구 > 옵션 > Xamarin > 라이브 다시 로드** (호스트 이름, 포트 및 암호화 키)의 정보는 앱에 포함 되므로,를 `LiveReload.Init()` 실행할 때 연결이 성공 하는 데는 페어링 또는 구성이 필요 하지 않습니다.

일반적인 네트워킹 문제 (방화벽, 다른 네트워크의 장치) 외에는 앱이 Visual Studio의 구성과 다르기 때문에 앱이 성공적으로 IDE를 연결 하지 못하는 주된 이유가 있습니다. 이 문제는 다음과 같은 경우에 발생할 수 있습니다.

* 앱이 다른 컴퓨터에서 컴파일 되었습니다.
* 앱이 다른 Visual Studio 세션에서 컴파일되고 배포 되었으며, 도구 > 옵션에서 **암호화 키 자동 생성** 이 선택 되어 있습니다 (기본값). **Xamarin > 라이브 다시 로드 >** 합니다.
* Visual Studio 설정이 변경 되었습니다 (즉, 호스트 이름, 포트 또는 암호화 키). 앱이 다시 빌드되고 배포 되지 않았습니다.

이러한 경우는 모두 앱을 빌드 및 배포 하 여 해결할 수 있습니다.

### <a name="uninstalling-preview-1"></a>Preview 1 제거

이전 미리 보기가 있고 제거 하는 데 문제가 있는 경우 다음 단계를 수행 합니다.

1. **C:\Program files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** 폴더를 삭제 합니다 (참고: "Enterprise"를 설치 된 버전으로 바꾸고, 안정적인 VS에 설치 된 경우 "Preview"를 "2017"으로 바꿉니다.
2. 해당 Visual Studio에 대 한 **개발자 명령 프롬프트** 를 열고를 실행 `devenv /updateconfiguration` 합니다. 

## <a name="tips--tricks"></a>팁 & 트릭

* 암호화 키 **자동 생성**을 해제 하는 경우 처럼 암호화 키를 포함 하 여 라이브 다시 로드 설정이 변경 되지 않고 동일한 컴퓨터에서 빌드하는 경우에는 코드 또는 종속성을 변경 하지 않는 한 초기 배포 후에 앱을 빌드 및 배포할 필요가 없습니다. 이전에 배포 된 앱을 다시 시작할 수 있으며 사용 된 마지막 호스트에 연결 됩니다.

* 동일한 Visual Studio 세션에 연결할 수 있는 장치 수에는 제한이 없습니다. 모든 장치에서 동시에 작업을 수행 하는 것을 확인 하기 위해 필요에 따라 많은 장치/시뮬레이터에서 앱을 배포 하 고 시작할 수 있습니다.

* 라이브 다시 로드는 앱의 사용자 인터페이스 부분만 다시 로드 하지만 페이지를 다시 만들지 *않고* 뷰 모델 (또는 바인딩 컨텍스트)을 대체 하지 않습니다. 즉, *전체* 앱 상태는 삽입 된 종속성을 포함 하 여 다시 로드 간에 항상 유지 됩니다.

## <a name="live-reload-server"></a>서버 라이브 다시 로드

실행 중인 응용 프로그램에서 컴퓨터로의 연결 `localhost` `127.0.0.1` > ( **Xamarin > 라이브 다시 로드 > Xamarin 라이브 다시 로드**)을 사용할 수 없는 경우 (예: 방화벽, 다른 네트워크) 원격 서버를 구성할 수 있습니다 .이 경우에는 IDE와 앱 모두에 대 한 연결을 설정 하는 대신 원격 서버를 구성할 수 있습니다.

라이브 다시 로드는 표준 [Mqtt 프로토콜](https://mqtt.org/) 을 사용 하 여 메시지를 교환 하므로 [타사 서버](https://github.com/mqtt/mqtt.github.io/wiki/servers)와 통신할 수 있습니다. 사용할 수 있는 [공용 서버](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) ( *브로커*라고도 함)도 제공 됩니다. `broker.hivemq.com` `iot.eclipse.org` [Www.cloudmqtt.com](https://www.cloudmqtt.com) 및 [www.cloudamqp.com](https://www.cloudamqp.com)에서 제공 하는 서비스 뿐만 아니라 및 호스트 이름을 사용 하 여 실시간 다시 로드를 테스트 했습니다. Azure의 Azure에서 사용자 고유의 MQTT 서버를 배포할 수도 있습니다 (예: [Azure에서의 Hivemq](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)).

모든 포트를 구성할 수 있지만 원격 서버에 기본 1883 포트를 사용 하는 것이 일반적입니다. 라이브 다시 로드 메시지는 강력한 종단 간 AES 대칭 암호화를 사용 하므로 원격 서버에 연결 하는 것이 안전 합니다. 기본적으로 모든 Visual Studio 세션에서 암호화 키와 IV (initialization vector)가 다시 생성 됩니다.

가장 쉬운 방법은 Azure에서 빈 Ubuntu VM에 [mosquitto](https://mosquitto.org) 서버를 설치 하는 것입니다.

1. Azure Portal에서 새 Ubuntu Server VM 만들기
2. 네트워킹 탭에서 1883 (기본 MQTT 포트)에 대 한 새 인바운드 포트 규칙을 추가 합니다.
3. [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash 모드)를 엽니다.
4. `ssh [USERNAME]@[PUBLIC_IP]`1) 및 VM 개요 페이지에 표시 된 공용 IP를 사용 하 여 입력 합니다.
5. `sudo apt-get install mosquitto`을 실행 하 고 1에서 선택한 암호를 입력 합니다.

이제 해당 IP를 사용 하 여 사용자 고유의 MQTT 서버에 연결할 수 있습니다.
