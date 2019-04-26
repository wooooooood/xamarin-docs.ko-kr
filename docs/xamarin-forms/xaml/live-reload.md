---
title: Xamarin 라이브 다시 로드 (미리 보기)
description: XAML에 변경 내용을 다른 컴파일 없이 실시간으로 반영 되었는지 확인 하 고 배포 합니다.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
robots: noindex
ms.date: 10/26/2018
ms.openlocfilehash: 21ff09f2af93ee46578b959111bf744ba05a74d7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384935"
---
# <a name="xamarin-live-reload-preview"></a>Xamarin 라이브 다시 로드 (미리 보기)

> [!NOTE]
> Xamarin 라이브 다시 로드의 미리 보기 기간이 종료 하 고 모든 사용자에 게 사용자 의견과 감사 하려고 합니다. 읽어보세요 우리의 [로드맵](https://docs.microsoft.com/visualstudio/productinfo/vs-roadmap) Xamarin.Forms에 대 한 현재 작업 중인 새로운 생산성 기능에 자세히 알아보려면 Visual Studio 2019에 대 한 합니다. 이 확장 Visual Studio 2017에 대 한 사용 가능한 상태로 유지 되지만 향후 업데이트 받고 있지 않습니다.

Xamarin 라이브 다시 로드 수 있습니다 **에 XAML을 변경 하 고 다른 컴파일 없이 실시간으로 반영 배포**합니다. XAML을 변경하고 저장하면 배포 대상에 다시 반영됩니다.

## <a name="requirements"></a>요구 사항

* [Visual Studio 2017 버전 15.7 이상을](https://visualstudio.microsoft.com/vs/) 사용 하 여 합니다 **.NET을 사용한 모바일 개발** 워크 로드.
* [Xamarin.Forms 3.0.0 이상을](https://www.nuget.org/packages/Xamarin.Forms/)합니다.

## <a name="getting-started"></a>시작
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Visual Studio 마켓플레이스에서 Xamarin Live Reload 설치

Xamarin Live Reload는 Visual Studio 마켓플레이스를 통해 배포됩니다. 확장을 설치하려면 [Visual Studio 마켓플레이스의 Xamarin Live Reload 페이지](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) 웹 사이트를 방문해 **다운로드**를 클릭합니다.

다운로드가 완료되면 .vsix를 클릭해 열고 **설치**합니다.

![Visual Studio 설치 관리자에서 Xamarin Live Reload 체크](images/LiveReloadVSIXInstall.png)

또는 Visual Studio에서 **확장명 및 업데이트** 대화상자의 **온라인** 탭에서 찾을 수 있습니다.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Xamarin Live Reload를 사용하도록 앱 구성

기존 모바일 앱에 3단계로 Live Reload를 추가할 수 있습니다.

1. 모든 프로젝트가 [Xamarin.Forms 3.0.0 이상](https://www.nuget.org/packages/Xamarin.Forms/)인지 확인.

2. **Xamarin.LiveReload** NuGet 패키지 추가:

    a. **.NET Standard** – **Xamarin.LiveReload** NuGet을 .NET Standard 2.0 라이브러리에 설치합니다. 이 Nuget은 플랫폼 프로젝트에 따로 설치할 필요는 없습니다. **패키지 소스**가 **모두**로 설정되어있는지 확인합니다.
    
    b. **공유 프로젝트** – **Xamarin.LiveReload** NuGet을 모든 플랫폼 프로젝트(Android, iOS, UWP 등)에 설치합니다. **패키지 소스**가 **모두**로 설정되어있는지 확인합니다.

    [![NuGet 패키지 관리자를 사용 하 여 Xamarin 라이브 다시 로드 NuGet 추가](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. 아래 코드 조각에 있는 것처럼 `Application` 클래스의 생성자에 `LiveReload.Init();` 추가:

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

### <a name="3-start-live-reloading"></a>3. Live Reload 시작

앱을 컴파일 및 배포합니다. 앱이 배포되면 XAML 파일을 열고, 일부를 변경한 후 파일을 저장합니다. 변경 내용이 배포 대상에 다시 배포됩니다.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live Reload는 작동하는 모든 XAML 파일에서 사용할 수 있습니다. C#이나 NuGet 패키지의 추가/제거는 다시 빌드하고 배포해야 변경 사항을 적용할 수 있습니다.

## <a name="frequently-asked-questions"></a>질문과 대답 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Mac 용 Xamarin 라이브 다시 로드를 Visual Studio에서 사용할 수 있는? 

아니요, Xamarin 라이브 다시 로드의 미리 보기 릴리스는 Visual Studio 2017에 대 한 사용 가능한만 합니다.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>Prism 등의 모든 라이브러리를 사용 하 여이 작동 합니까? 

앱은 컴파일되므로 라이브 다시 로드 Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity, 및 기타 컨트롤 공급 업체 등 Prism, 타사 컨트롤 라이브러리 등 모든 라이브러리를 사용 하 여 작동 합니다.

### <a name="what-changes-does-live-reload-redeploy"></a>라이브 다시 로드에는 어떤 변경 재배포는? 

실시간 다시 로드에는 XAML 또는 CSS에 대 한 변경만 적용 됩니다. C# 파일을 변경한 경우에 recompile 해야 합니다. 

### <a name="what-platforms-are-supported"></a>어떤 플랫폼이 지원 되나요? 

실시간 다시 로드, Android, iOS 및 UWP를 포함 하 여 Xamarin.Forms에서 지 원하는 모든 플랫폼에서 작동 합니다.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>실제 장치, 에뮬레이터 및 시뮬레이터에이 작동 합니까? 

예, 라이브 다시 로드 Android 에뮬레이터, iOS 시뮬레이터 및 물리적 장치를 비롯 한 모든 유효한 배포 대상에 작동 합니다. 장치 및 컴퓨터 같은 Wi-fi 네트워크에 되어 있어야 하는 장치에 배포 합니다.

### <a name="does-this-work-with-corporate-networks"></a>회사 네트워크를 사용 하 여이 작동 합니까?

IOS 시뮬레이터는 Android 에뮬레이터를 디버깅 하는 경우 라이브 다시 로드 localhost를 사용 하 여 통신 합니다. 장치에 배포 하려는 경우 장치 및 컴퓨터 같은 Wi-fi 네트워크에 배치 해야 해야 합니다. 가능 하지 않은 시나리오를 수행할 수 있습니다 [라이브 다시 로드 하는 서버 구성](#live-reload-server)가 수 있는 라이브 다시 로드 하려면 네트워크 연결 설정에 관계 없이 합니다.

### <a name="does-it-require-debugging-the-app"></a>앱을 디버깅 해야 하나요? 

아니요. 실제로 임의 개수의 장치 또는 시뮬레이터/에뮬레이터에서 모든 지원 되는 응용 프로그램의 대상 (Android, iOS 및 UWP) 시작 고를 모두 한 번에 업데이트를 볼 수 있습니다. 

## <a name="limitations"></a>제한 사항

* XAML의 다시 로드만 지원 됩니다.
* MVVM을 사용 하지 않는 UI 상태 간에 재배포를 관리할 수 있습니다.

## <a name="known-issues"></a>알려진 문제

* Visual Studio 에서만 지원 됩니다.
* 연결 설정 해야 합니다 **연결 하지 않음** 또는 **프레임 워크 Sdk를 전용에 링크** 
* 앱 전체 리소스를 다시 로드 (예: **App.xaml** 또는 리소스가 공유), 앱 탐색을 다시 설정 됩니다. 
* 현재 ContentView 다시 로드를 포함 하는 페이지를 다시 로드 해야 합니다.
* AutomationId를 포함 하는 요소는 다시 로드 오류가 발생할 수 있습니다.
* 런타임 크래시가 발생할 수 있습니다 UWP를 디버깅 하는 동안 XAML을 편집 합니다. 해결 방법: 사용 하 여 **(Ctrl + F5) 디버깅 하지 않고 시작** of **디버깅 시작 (F5)** 합니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-codes"></a>오류 코드

* **XLR001**: *현재 프로젝트 'Xamarin.LiveReload' NuGet 패키지 버전 '[VERSION]'를 참조 하지만, Xamarin 라이브 다시 로드 확장 버전 '[VERSION]'이 필요 합니다.*

  라이브 다시 로드 기능의 발전와 신속 하 게 반복을 허용 하기 위해 nuget 패키지와 Visual Studio 확장명 정확히 일치 해야 합니다. Nuget 패키지를 설치한 확장의 동일한 버전으로 업데이트 합니다.

* **XLR002**: *실시간 다시 로드 명령줄에서 빌드할 때 'MqttHostname' 속성 이상이 필요 합니다. 또는 'EnableLiveReload' 기능을 사용 하지 않도록 설정 하려면 'false'를 설정 합니다.*

  라이브 다시 로드에 필요한 속성을 사용할 수 없는 경우 명령줄에서 (또는 연속 통합) 빌드 및 따라서 명시적으로 제공 해야 합니다. 

* **XLR003**: *라이브 다시 로드 nuget 패키지는 Xamarin 라이브 다시 로드 Visual Studio 확장을 설치 해야 합니다.*

  라이브 다시 로드 nuget 패키지를 참조 하는 프로젝트 빌드를 시도 하지만 Visual 확장이 설치 되어 있지.  

* *어셈블리를 로드 하는 동안 예외가 발생 합니다. System.IO.FileNotFoundException: 어셈블리를 로드할 수 없습니다 ' Xamarin.Live.Reload, 버전 0.3.27.0, Culture = neutral, PublicKeyToken = ='.*

  호스트 프로젝트를 사용 해야 `PackageReference` 대신 `packages.config`

### <a name="app-doesnt-connect"></a>앱에 연결 하지 않습니다.

응용 프로그램을 빌드할 때 정보를 **도구 > 옵션 > Xamarin > 라이브 다시 로드** 있으므로 앱에 포함 됩니다 (호스트 이름, 포트 및 암호화 키) 때 `LiveReload.Init()` 없습니다 쌍 또는 구성 실행 되는 연결이 성공 하려면에 대 한 필요 합니다.

앱 IDE를 성공적으로 연결할 수 있습니다 하는 주된 이유는 표준 네트워킹 문제 (방화벽, 다른 네트워크 장치), 제외 Visual Studio에서 해당 구성을 다르므로입니다. 이 경우에 발생할 수 있습니다.

* 앱은 다른 컴퓨터에서 컴파일 되었습니다.
* 앱을 컴파일 했 고 다른 Visual Studio 세션에서 배포 및 **암호화 키를 자동으로 생성** 됩니다 (기본값)를 체크 **도구 > 옵션 > Xamarin > 라이브 다시 로드**합니다.
* Visual Studio 설정 된 (즉, 호스트 이름, 포트 또는 암호화 키)를 변경 하지만 앱 빌드되고 다시 배포 하지 되었습니다.

이러한 경우 모두 빌드하고 앱을 다시 배포 하 여 해결 됩니다.

### <a name="uninstalling-preview-1"></a>미리 보기 1 제거

이전 미리 보기를 있고 제거 하는 데 문제가 있는 경우 다음이 단계를 수행 합니다.

1. 폴더를 삭제할 **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (참고: 설치 된 버전을 사용 하 여 "Enterprise"으로 바꾸고 "Preview" "2017" 경우 있습니다 안정적인 vs 설치)
2. 엽니다는 **개발자 명령 프롬프트** 는 Visual Studio 및 실행에 대 한 `devenv /updateconfiguration`합니다. 

## <a name="tips--tricks"></a>팁과 트릭

* 와 라이브 다시 로드 설정을 변경 하지 마세요 (암호화 키를 포함 하 여 경우와 같이 기능을 해제 하면 **암호화 키를 자동으로 생성**) 동일한 컴퓨터에서 빌드를 빌드 및 초기 후 앱을 배포할 필요가 없습니다 코드 또는 종속성을 변경 하지 않는 한 배포 합니다. 시작할 수 있습니다만 다시 이전에 배포한 앱을 하 고 사용 되는 호스트를 마지막으로 연결 됩니다.

* 동일한 Visual Studio 세션에 연결할 수는 얼마나 많은 장치에 제한이 없습니다. 배포를 한 번에 모든 라이브 다시 로드 작업을 참조 하는 데 필요한 만큼 장치/시뮬레이터에서 앱을 시작 합니다.

* 실시간 다시 로드는 앱의 사용자 인터페이스 부분에만 다시 로드 하지만 *되지* 하 여 페이지를 다시 만들고, 모두는 대체 보기 모델 (또는 바인딩 컨텍스트). 즉, 합니다 *전체* 응용 프로그램 상태는 항상 다시 로드를 삽입 된 종속성을 포함 하 여 간에 유지 됩니다.

## <a name="live-reload-server"></a>실시간 다시 로드 서버

시나리오에서 실행 중인 앱에서 컴퓨터 연결이 (사용 하 여 표시 된 대로 `localhost` 또는 `127.0.0.1` 에서 **도구 > 옵션 > Xamarin > 라이브 다시 로드**) 수는 없습니다 (즉, 방화벽, 서로 다른 네트워크) 구성할 수 있습니다 원격 서버 대신, IDE 및 앱 모두를 conect 하 합니다.

실시간 다시 로드는 표준을 [MQTT 프로토콜](http://mqtt.org/) 에 메시지를 교환 하 고 따라서 통신할 수 있습니다 [타사 서버](https://github.com/mqtt/mqtt.github.io/wiki/servers)합니다. 도 있습니다 [공용 서버](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (라고도 *브로커*) 사용할 수 있는 합니다. 라이브 다시 로드 테스트 되었습니다 `broker.hivemq.com` 하 고 `iot.eclipse.org` 에서 제공 하는 서비스 뿐만 아니라 호스트 이름을 [www.cloudmqtt.com](https://www.cloudmqtt.com) 하 고 [www.cloudamqp.com](https://www.cloudamqp.com). 클라우드에서 고유한 MQTT 서버와 같은 배포할 수도 있습니다 [Azure에서 HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)합니다.

모든 포트를 구성할 수 있지만 일반적으로 원격 서버에 대 한 기본 1883 포트를 사용 하는 것입니다. 원격 서버에 연결 되어 있으므로 라이브 다시 로드 메시지에서 강력한 엔드-투-엔드 AES 대칭 암호화를를 사용 합니다. 기본적으로 암호화 키와 초기화 벡터 (IV) 모든 Visual Studio 세션에서 다시 생성 됩니다.

아마도 가장 쉬운 설치 하는 것은 [mosquitto](https://mosquitto.org) Azure에서 빈 Ubuntu VM에는 서버:

1. Azure Portal에서 새 Ubuntu Server VM 만들기
2. 네트워킹 탭에 1883 (기본 MQTT 포트)에 대 한 새 인바운드 포트 규칙 추가
3. 엽니다는 [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash 모드)
4. 입력 `ssh [USERNAME]@[PUBLIC_IP]` 1에서 선택한 사용자를 사용 하 여) 및 VM 개요 페이지에 표시 된 공용 IP
5. 실행 `sudo apt-get install mosquitto`, 1에서 선택한 암호를 입력)

이제 고유한 MQTT 서버에 연결 하는 IP를 사용할 수 있습니다.
