---
title: 라이브 다시 로드
description: XAML에 대 한 변경 다른 컴파일 필요 없이 실시간으로 반영 확인 하 고 배포 합니다.
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 05/11/2018
ms.openlocfilehash: 756f0570ce792450cfcaf6b1c5161a95a6cb80c8
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848306"
---
# <a name="xamarin-live-reload"></a>Xamarin 라이브 다시 로드

![미리 보기](~/media/shared/preview.png)

Xamarin Live Reload는 **XAML을 변경 할 때 따로 컴파일 혹은 배포 과정 없이 실시간으로 변경 점을 확인할 수** 있습니다. XAML을 변경하고 저장하면 배포 대상에 다시 반영됩니다.

Live Reload를 사용할 때 앱이 컴파일 되기 때문에, 모든 라이브러리와 타사 컨트롤에서도 작동 합니다. iOS, Android, UWP 등 Xamarin.Forms이 지원하는 모든 플랫폼들을 지원하며 시뮬레이터, 에뮬레이터 및 물리적 장치 등 모든 유효한 배포 대상에서 사용할 수 있습니다.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live Reload는 현재 Visual Studio 2017에서만 사용할 수 있습니다.

## <a name="requirements"></a>요구 사항

* [Visual Studio 2017 15.7 버전 이상](https://www.visualstudio.com/vs/) 이상의 **.NET을 사용한 모바일 개발** 작업.
* [Xamarin.Forms 3.0.0 이상](https://www.nuget.org/packages/Xamarin.Forms/) 이상.

## <a name="getting-started"></a>시작
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Visual Studio 마켓플레이스에서 Xamarin Live Reload 설치

Xamarin Live Reload는 Visual Studio 마켓플레이스를 통해 배포 됩니다. 확장을 설치 하려면 [Visual Studio 마켓플레이스의 Xamarin Live Reload 페이지](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) 웹 사이트를 방문 해  **다운로드**를 클릭 합니다.

다운로드가 완료되면 .vsix를 클릭해 열고 **설치**합니다.

![Visual Studio 설치 관리자에서 Xamarin Live Reload 체크](images/LiveReloadVSIXInstall.png)

또는 Visual Studio에서 **확장명 및 업데이트** 대화상자의 **온라인** 탭에서 찾을 수 있습니다.

### <a name="2-configure-your-app-to-use-live-reload"></a>2. Xamarin Live Reload를 사용 하도록 앱 구성

기존 모바일 앱에 3단계로 Live Reload를 추가할 수 있습니다.

1. 모든 프로젝트가 [Xamarin.Forms 3.0.0 이상](https://www.nuget.org/packages/Xamarin.Forms/) 인지 확인.

2. **Xamarin.LiveReload** NuGet 패키지 추가:

    a. **.NET Standard** – **Xamarin.LiveReload** NuGet을 .NET Standard 2.0 라이브러리에 설치합니다. 이 Nuget은 플랫폼 프로젝트에 따로 설치할 필요는 없습니다. **패키지 소스**가 **모두**로 설정 되어있는 지 확인합니다.
    
    b. **공유 프로젝트** – *Xamarin.LiveReload** NuGet을 모든 플랫폼 프로젝트(Android, iOS, UWP 등)에 설치합니다. **패키지 소스**가 **모두**로 설정 되어있는 지 확인합니다.

    [![Xamarin 라이브 다시 로드 NuGet NuGet 패키지 관리자를 추가 합니다.](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. `Application`의 생성자에 아래 코드 조각에 있는 것 처럼 `LiveReload.Init();`를 추가 합니다.

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

앱을 컴파일 및 배포 합니다. 앱이 배포 된 후을 XAML 파일을 열고, 일부를 변경해 보고 및 파일을 저장 합니다. 변경 내용이 배포 대상에 다시 배포 됩니다.

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

Live Reload는 작동 하는 모든 XAML 파일에서 사용할 수 있습니다. C#이나 NuGet 패키지의 추가/제거는 다시 빌드하고 배포해야 변경 점을 적용할 수 있습니다.

## <a name="frequently-asked-questions"></a>질문과 대답 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Mac 용 Xamarin 라이브 다시 로드를 Visual Studio에서 사용할 수 있는? 

Xamarin 라이브 다시 로드의 초기 미리 보기 릴리스는 Visual Studio 2017 수만 있습니다. Mac 용 Visual Studio에 대 한 지원이 향후 릴리스에 예정입니다.

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>프리즘 같은 모든 라이브러리와이 작동 합니까? 

응용 프로그램은 컴파일되므로 라이브 다시 로드 프리즘, 타사 컨트롤 라이브러리 등과 같이, Telerik, Infragistics, Syncfusion, ArcGIS, GrapeCity, 및 기타 컨트롤 공급 업체와 같은 모든 라이브러리와 함께 작동 합니다.

### <a name="what-changes-does-live-reload-redeploy"></a>변경 내용을 라이브 다시 로드를 다시 배포? 

라이브 다시 로드에는 XAML 나 CSS 변경만 적용 됩니다. C# 파일을 변경한 경우 다시 컴파일한 해야 합니다. 이후 버전에 대 한 다시 로드 하 고 C#에 대 한 지원이 예정 되어 합니다.

### <a name="what-platforms-are-supported"></a>플랫폼 지원 되나요? 

Android, iOS 및 UWP를 포함 하 여 Xamarin.Forms에서 지 원하는 모든 플랫폼에서 작동 하는 라이브 다시 로드 합니다.

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>물리적 장치, 에뮬레이터 및 시뮬레이터 등에이 작동 합니까? 

예, 라이브 다시 로드 Android 에뮬레이터, iOS 시뮬레이터 및 물리적 장치를 비롯 한 모든 유효한 배포 대상에 작동 합니다. 장치에 배포 하려면 장치와 컴퓨터를 동일한 Wi-fi 네트워크에 높아야 합니다.

### <a name="does-this-work-with-corporate-networks"></a>회사 네트워크와이 작동 합니까?

Android 에뮬레이터 또는 iOS 시뮬레이터를 디버그 하는 경우 라이브 다시 로드 통신 하기 위해 localhost를 사용 합니다. 장치에 배포 하려는 경우 장치 및 컴퓨터에 있이 필요가 동일한 Wi-fi 네트워크입니다. 수 없는 이것이 불가능 시나리오에서는 [라이브 다시 로드 하는 서버 구성](#live-reload-server), 네트워크 연결 설정에 관계 없이 라이브 다시 로드 하려면 수는 있는 합니다.

### <a name="does-it-require-debugging-the-app"></a>응용 프로그램을 디버깅 없어도? 

아니요. 사실,도 모든 장치 또는 시뮬레이터/에뮬레이터에 프로그램 모든 지원 되는 응용 프로그램 대상 (Android, iOS 및 UWP) 시작 고를 모두 한 번에 업데이트를 확인할 수 있습니다. 

## <a name="limitations"></a>제한 사항

* XAML의 다시 로드만 지원 됩니다.
* UI 상태 MVVM 사용 하지 않는 유지 재배포 시, 사이 수 있습니다.

## <a name="known-issues"></a>알려진 문제

* Visual Studio에서 에서만 지원 됩니다.
* 응용 프로그램 수준의 리소스를 다시 로드 (즉, **App.xaml** 또는 공유 리소스 사전), 응용 프로그램 탐색을 다시 설정 됩니다. 이 다음 미리 보기 릴리스에서 수정 될 예정입니다.
* UWP 디버깅 인해 런타임 충돌 하는 동안 XAML을 편집 합니다. 해결 방법: 사용 **(Ctrl + F5) 디버깅 하지 않고 시작** 대신 **디버깅 시작 (F5)** 합니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="error-codes"></a>오류 코드

* **XLR001**: *Xamarin 라이브 다시 로드 확장 버전 [VERSION]에 필요 하지만 현재 프로젝트 참조 'Xamarin.LiveReload' NuGet 패키지 버전 [VERSION].*

  신속 하 게 반복 및 라이브 다시 로드 기능의 변화를 허용 하려면 nuget 패키지와 Visual Studio 확장명 정확히 일치 해야 합니다. Nuget 패키지를 설치한 후 확장의 동일한 버전으로 업데이트 합니다.

* **XLR002**: *명령줄에서 빌드할 때 다시 로드 라이브 필요 이상 'MqttHostname' 속성입니다. 또는 'EnableLiveReload' 기능을 비활성화 하는 'false'를 설정 합니다.*

  라이브 다시 로드 하 여 필요한 속성 사용할 수 없는 경우 명령줄에서 (또는 연속 통합), 빌드 및 따라서 명시적으로 제공 해야 합니다. 

* **XLR003**: *nuget 패키지를 다시 로드 라이브 Xamarin 라이브 다시 로드 Visual Studio 확장을 설치 해야 합니다.*

  다시 로드 라이브 nuget 패키지를 참조 하는 프로젝트를 빌드하려고 시도 하지만 Visual 확장 설치 되어 있지 않습니다.  

* *어셈블리를 로드 하는 동안 예외: System.IO.FileNotFoundException: 어셈블리를 로드할 수 없습니다 ' Xamarin.Live.Reload, 버전 0.3.27.0, Culture = neutral, PublicKeyToken = ='.*

  호스트 프로젝트를 사용 해야 `PackageReference` 대신 `packages.config`

### <a name="app-doesnt-connect"></a>응용 프로그램에 연결 하지 않습니다.

응용 프로그램 빌드할 때, 정보를 **도구 > 옵션 > Xamarin > 라이브 다시 로드** 하므로 (호스트 이름, 포트 및 암호화 키)는 응용 프로그램에 포함 됩니다 때 `LiveReload.Init()` 없습니다 쌍 또는 구성 실행은 연결이 성공 하려면에 대 한 필요 합니다.

응용 프로그램 IDE를 성공적으로 연결 될 수는 주요 이유는 표준 네트워킹 문제 (방화벽, 다른 네트워크 장치), 제외 구성을 Visual Studio의 다르기 때문에입니다. 이 경우에 발생할 수 있습니다.

* 응용 프로그램은 다른 컴퓨터에서 컴파일 되었습니다.
* 응용 프로그램을 컴파일 했 고 다른 Visual Studio 세션에서 배포 및 **암호화 키를 자동으로 생성** 됩니다 (기본값) 체크 인 **도구 > 옵션 > Xamarin > 라이브 다시 로드**합니다.
* Visual Studio 설정이 (즉, 호스트 이름, 포트 또는 암호화 키) 변경 되었습니다. 하지만 앱 빌드 및 다시 배포 하지 않습니다.

이러한 경우 모두 빌드하고 응용 프로그램을 다시 배포 하 여 해결 됩니다.

### <a name="uninstalling-preview-1"></a>미리 보기 1를 제거합니다.

이전 미리 보기를 있고 제거 하는 데 문제가 있으면 다음이 단계를 따르십시오.

1. 폴더 삭제 **C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (참고: "Enterprise"에 설치 된 버전으로 바꾸고 "미리 보기"와 "2017" if 있습니다 안정적인 VS에 설치)
2. 열기는 **개발자 명령 프롬프트** 해당 Visual Studio와 실행에 대 한 `devenv /updateconfiguration`합니다. 

## <a name="tips--tricks"></a>팁과 트릭

* 라이브 다시 로드 설정을 변경 하지 않는 상태로 (암호화 키를 포함 한 경우와 같이 해제 하면 **암호화 키를 자동으로 생성할**)와 같은 컴퓨터에서 빌드, 빌드 및 초기 후 앱을 배포할 필요가 없습니다 코드 또는 종속성을 변경 하지 않는 한 배포 합니다. 시작할 수 있습니다만 다시 이전에 배포 된 앱 및 호스트 사용을 마지막으로 연결 됩니다.

* 같은 Visual Studio 세션에 연결할 수 있습니다 하는 장치 수에 제한이 없습니다. 배포할 수 있으며 동시에 모든 속성에 라이브 다시 로드 작업을 참조 하는 데 필요한 만큼 장치/시뮬레이터에서 앱을 시작할 수 있습니다.

* 하지만 라이브 다시 로드 응용 프로그램의 사용자 인터페이스 부분을 다시 로드만 *하지* 둘 다는 것을 대체 뷰 모델 (또는 바인딩 컨텍스트), 페이지를 다시 만듭니다. 즉,는 *전체* 삽입 된 종속성을 포함 하 여 다시 로드할 필요가 없고에서 응용 프로그램 상태가 항상 유지 합니다.

## <a name="live-reload-server"></a>라이브 다시 로드 서버

시나리오에서 실행 중인 응용 프로그램에서 컴퓨터 연결이 (사용 하 여 표시 된 대로 `localhost` 또는 `127.0.0.1` 에 **도구 > 옵션 > Xamarin > 라이브 다시 로드**) 수는 없습니다 (즉, 방화벽, 서로 다른 네트워크) 구성할 수 있습니다는 원격 서버 대신 있으며 IDE와 응용 프로그램 모두에 conect 합니다.

표준을 사용 하 여 라이브 다시 로드 [MQTT 프로토콜](http://mqtt.org/) 에 메시지를 교환 하 고 따라서 통신할 수 있는 [타사 서버](https://github.com/mqtt/mqtt.github.io/wiki/servers)합니다. 까지 있습니다 [공용 서버](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (라고도 *브로커*) 사용할 수 있는 합니다. 라이브 다시 로드에서 테스트 되었지만 `broker.hivemq.com` 및 `iot.eclipse.org` 으로 호스트 이름을 제공 하는 서비스 [www.cloudmqtt.com](https://www.cloudmqtt.com) 및 [www.cloudamqp.com](https://www.cloudamqp.com)합니다. 배포할 수도 있습니다는 클라우드에서 사용자 고유의 MQTT 서버와 같은 [Azure에서 HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)합니다.

모든 포트를 구성할 수 있지만 일반적으로 원격 서버에 대 한 기본 1883 포트를 사용 하 합니다. 다시 로드 메시지 강력한 종단 간 AES 대칭 암호화를 사용 하 여 원격 서버에 연결 하는 데 안전 하 게 보호 되기 때문에 존재 합니다. 기본적으로 암호화 키와 초기화 벡터 (IV) 모든 Visual Studio 세션에서 다시 생성 됩니다.

아마도 가장 쉬운 방법은 설치 하는 것은 [mosquitto](https://mosquitto.org) 서버에 Azure에서 빈 Ubuntu VM:

1. Azure 포털에서 새 Ubuntu Server VM을 만듭니다.
2. 네트워크 탭에서 1883 (기본 MQTT 포트)에 대 한 새 인바운드 포트 규칙을 추가 합니다.
3. 열기는 [클라우드 셸](https://docs.microsoft.com/azure/cloud-shell/overview) (이용한 적 모드)
4. 입력 `ssh [USERNAME]@[PUBLIC_IP]` 1에서 선택한 사용자 이름을 사용 하 여) 및 VM 개요 페이지에 표시 된 공용 IP
5. 실행 `sudo apt-get install mosquitto`, 1에서 선택한 암호를 입력)

이제 고유한 MQTT 서버에 연결 하는 데 해당 IP를 사용할 수 있습니다.
