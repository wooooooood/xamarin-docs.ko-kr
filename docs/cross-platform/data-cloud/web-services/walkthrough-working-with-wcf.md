---
title: 연습-WCF 작업
description: 이 연습에서는 Xamarin으로 빌드된 모바일 응용 프로그램 수 BasicHttpBinding 클래스를 사용 하 여 WCF 웹 서비스를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: b0502028985cf35498a5811ec88b9e32d74d0311
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33918866"
---
# <a name="walkthrough---working-with-wcf"></a>연습-WCF 작업

_이 연습에서는 Xamarin으로 빌드된 모바일 응용 프로그램 수 BasicHttpBinding 클래스를 사용 하 여 WCF 웹 서비스를 사용 하는 방법을 설명 합니다._


것은 모바일 응용 프로그램을 백 엔드 시스템과 통신할 수에 대 한 일반적인 요구 사항입니다. 많은 선택 항목 및 그 중 하나는 백 엔드 프레임 워크에 대 한 옵션 [Windows Communication Foundation](http://msdn.microsoft.com/library/ms731082.aspx) (WCF). 이 연습에서는 어떻게 Xamarin 모바일 응용 프로그램을 사용 하 여 WCF 서비스 소비할 수 있는의 예로 제공는 `BasicHttpBinding` 클래스입니다. 이 연습에서는 다음 항목을 다룹니다.

1.  **WCF 서비스 만들기** -이 섹션에 있는 두 가지 방법으로 매우 기본적인 WCF 서비스 만듭니다. 첫 번째 메서드는 C# 개체를 수행 하는 또 다른 방법은 동안 문자열 매개 변수를, 걸립니다. 이 섹션에는 WCF 서비스에 대 한 원격 액세스를 허용 하는 개발자의 워크스테이션을 구성 하는 방법을 설명도 합니다.
1.  **Xamarin.Android 응용 프로그램 만들기** -WCF 서비스를 만든 후 WCF 서비스를 사용 하는 간단한 Xamarin.Android 응용 프로그램을 만듭니다. 이 섹션은 WCF 서비스와의 통신을 용이 하 게 하려면 WCF 서비스 프록시 클래스를 만드는 방법을 설명 합니다.
1.  **Xamarin.iOS 응용 프로그램 만들기** -이 자습서의 마지막 부분에서는 WCF 서비스를 사용 하는 간단한 Xamarin.iOS 응용 프로그램을 만들어야 합니다.

<a name="Requirements" />

## <a name="requirements"></a>요구 사항

이 연습에서는 WCF 서비스 만들기 및 사용을 잘 알고 있다고 가정 합니다.

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>WCF 서비스 만들기

Us 하기 전에 첫 번째 작업와 통신 하는 모바일 응용 프로그램에 대 한 WCF 서비스를 만드는 것입니다.

1. Visual Studio 2017을 시작 하 고 새 프로젝트를 만듭니다.
1. 에 **새 프로젝트** 대화 상자에서 선택 된 **WCF > WCF 서비스 라이브러리** 템플릿과 솔루션 이름을 `HelloWorldService`:

    ![](walkthrough-working-with-wcf-images/new-wcf-service.png "새 WCF 서비스 라이브러리 만들기")

1. **솔루션 탐색기**, 라는 새 클래스를 추가 `HelloWorldData` 프로젝트에:

    ```csharp
        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }
    ```


1. **솔루션 탐색기**, 이름 바꾸기 `IService1.cs` 를 `IHelloWorldService.cs`, 이름 바꾸기 및 `Service1.cs` 를 `HelloWorldService.cs`합니다.
1. **솔루션 탐색기**개방형 `IHelloWorldService.cs` 코드를 다음 코드로 바꿉니다.

    ```csharp
        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }
    ```
  
    이 서비스에서는 두 가지 방법-매개 변수 및 다른에 대 한 문자열을 사용 하는.NET 개체를 사용 합니다.

1. **솔루션 탐색기**개방형 `HelloWorldService.cs` 코드를 다음 코드로 바꿉니다.

    ```csharp
        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }
    ```

1. **솔루션 탐색기**개방형 `App.config`, 업데이트는 `name` 특성의는 `<service>` 노드를는 `contract` 특성에는 `<endpoint>` 노드를 및 `baseAddress` 는 의특성`<add>` 노드:

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>
    ```

1. 빌드하고 WCF 서비스를 실행 합니다. 서비스는 WCF 테스트 클라이언트에서 호스트 됩니다.

    ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "테스트 클라이언트에서 실행 되는 WCF 서비스")

1. WCF 테스트 클라이언트 실행을 브라우저 시작을 WCF 서비스에 대 한 끝점으로 이동 합니다.

    ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "WCF 서비스 브라우저 정보 페이지")

> [!IMPORTANT]
> 다음 섹션은 Windows 10 워크스테이션에서 원격 연결을 허용 하는 경우 필요한 수만 있습니다. WCF 서비스를 배포할 수 있는 대체에 플랫폼을 사용 하는 경우에 섹션을 무시할 수 있습니다.

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>IIS Express에 대 한 원격 액세스 구성

연결만 로컬 컴퓨터에서 제공 되 면 로컬로 WCF를 호스트 하는 것은 적절 한 합니다. 그러나 원격 장치 (예: Android 장치 또는 iPhone) 로컬 WCF 서비스에 대 한 액세스를 체계가 없습니다. 따라서이 섹션에서는 원격 연결을 허용 하도록 Windows 10 및 IIS Express를 구성 하는 방법을 설명 합니다.

1.  **수락 원격 연결에 IIS Express를 구성** -이 단계에서는 특정 포트에서 원격 연결을 허용 하도록 IIS Express에 대 한 구성 파일을 편집 하 고 다음 들어오는 트래픽을 허용 하려면 IIS Express에 대 한 규칙을 설정 합니다.
1.  **Windows 방화벽 예외가 추가** -원격 응용 프로그램에서 WCF 서비스와 통신 하는 데 사용할 수 있는 Windows 방화벽을 통해 포트를 열어야 합니다.

    워크스테이션의 IP 주소를 알아야 합니다. 이 예에서는 우리의 워크스테이션에는 IP 주소 192.168.1.143 합니다.

1. 외부 요청 수신 하기 위해 IIS Express를 구성 하 여 시작 해 보겠습니다. IIS Express에 대 한 구성 파일을 편집 하 여 이렇게 하려면 `[solutiondirectory]\.vs\config\applicationhost.config`다음 스크린샷에 표시 된 것 처럼:

    [![](walkthrough-working-with-wcf-images/image05.png "이렇게 하려면 solutiondirectory.vsconfigapplicationhost.config에서 IIS Express에 대 한 구성 파일을 편집 하 여이 스크린 샷에 표시 된 것 처럼")](walkthrough-working-with-wcf-images/image05.png#lightbox)

    찾을 `site` 이름의 요소 `HelloWorldWcfHost`합니다. 다음 XML 조각 같은 같아야 합니다.

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>
    ```
 
    다른 항목 추가 해야 합니다 `binding` 8734 외부 트래픽을 위해 포트를 엽니다. 다음 XML을 추가 하는 `bindings` 요소를 사용자의 IP 주소와 IP 주소를 교체 합니다.

    ```xml
    <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
    ```
    
    이렇게 하면 IIS Express 모든 원격 IP 주소는 컴퓨터의 외부 IP 주소에서 8734 포트에서 HTTP 트래픽을 허용 하도록 구성 됩니다. 위 코드 조각에서는 IIS Express를 실행 하는 컴퓨터의 IP 주소는 192.168.1.143 가정 합니다. 변경 된 후의 `bindings` 요소는 다음과 같이 표시 됩니다.

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>
    ```

1. 다음으로, IIS Express를 구성 해야 8734 포트에서 들어오는 연결을 허용 합니다. 관리 명령 프롬프트를 시작 및이 명령을 실행 합니다.

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. 마지막 단계는 포트 8734 외부 트래픽을 허용 하도록 Windows 방화벽을 구성 하는 것입니다. 관리 명령 프롬프트에서 다음 명령을 실행 합니다.

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    이 명령은 Windows 10 워크스테이션와 동일한 서브넷에 모든 장치에서 8734 포트에서 들어오는 트래픽을 허용 됩니다.

다른 장치 또는 우리의 서브넷에 있는 컴퓨터에서 들어오는 연결을 수락 하는 IIS Express에서 호스트 되는 매우 기본적인 WCF 서비스를 만들었습니다. 응용 프로그램을 실행 하 고 방문 하 여이 테스트할 수 있습니다 `http://localhost:8733/Design_Time_Addresses/HelloWorldService/` 워크스테이션에 설치 하 고 `http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/` 서브넷의 다른 컴퓨터에서 합니다.

서비스 이며 실행을 유지 하려면 IIS Express를 허용 하려면 해제는 **편집 하며 계속 하기** 옵션 *프로젝트 속성 > 웹 > 디버거*합니다.

## <a name="creating-a-proxy-for-the-web-service"></a>웹 서비스에 대 한 프록시 만들기

응용 프로그램 서비스를 사용할 수 전에 WCF 서비스에 대 한 웹 서비스 프록시를 만들어야 합니다. 이렇게 하려면 다음을 수행합니다.

1. 라는.NET 표준 클래스 라이브러리 추가 `HelloWorldServiceProxy`, 프로젝트의 모든 클래스를 삭제 합니다.
1. 실행 된 `HelloWorldService` 프로젝트.
1. 와 `HelloWorldService` 실행 프로젝트에서 새 추가 **연결 된 서비스** 을 프로젝트에 사용 하는 **Microsoft WCF 웹 서비스 참조 공급자**합니다.
1. 에 **서비스 끝점** 탭은 **WCF 웹 서비스 참조 구성** 대화 상자에서 클릭는 **Discover** 단추, 삭제 `mex` 검색 된의 끝에서 끝점에는 **URI** 드롭다운 목록에서 입력 `HelloWorldServiceProxy` 로 **Namespace**를 클릭 하 고는 **다음** 단추입니다.
1. 에 **데이터 형식 옵션** 탭은 **WCF 웹 서비스 참조 구성** 대화 상자에서 기본값을 클릭 하 여는 **다음** 단추입니다.
1. 에 **클라이언트 옵션** 탭은 **WCF 웹 서비스 참조 구성** 대화 상자에서 확인는 **공용** 확인란을 선택 하 고 클릭는 **마침**  단추입니다.
1. 빌드는 `HelloWorldServiceProxy` 프로젝트.

> [!NOTE]
> Microsoft WCF 웹 서비스 참조 공급자를 사용 하 여 Visual Studio 2017에 프록시를 만드는 대신 ServiceModel Metadata 유틸리티 도구 (svcutil.exe)를 사용 하는 것입니다. 자세한 내용은 참조 [ServiceModel Metadata 유틸리티 도구 (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)합니다.

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Xamarin.Android 응용 프로그램 만들기

WCF 서비스 프록시 Xamarin.Android 응용 프로그램에 의해 다음과 같이 사용 될 수 있습니다.

1. Visual Studio에서 새로운 Android 프로젝트를 솔루션에 추가 하 고 이름을 `HelloWorld.Android`합니다.
1. 에 `HelloWorld.Android` 프로젝트에서에 대 한 참조를 추가 `HelloWorldServiceProxy` 프로젝트 및에 대 한 참조는 `System.ServiceModel` 네임 스페이스입니다.
1. **솔루션 탐색기**개방형 `Resources/layout/main.axml` 기존 XML 다음 XML로 바꿉니다.

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>
    ```
    
    다음 스크린샷에서 디자이너에서 UI를 보여 줍니다.

    [![](walkthrough-working-with-wcf-images/image09.png "이 디자이너에서이 UI 모양을의 스크린 샷")](walkthrough-working-with-wcf-images/image09.png#lightbox)
    
1. **솔루션 탐색기**개방형 `Resources/values/Strings.xml` 다음 XML을 추가 하 고:

    ```xml
    <string name="say_hello_world">Say Hello World</string>
    <string name="get_hello_world_data">Get Hello World data</string>
    ```
    
1. **솔루션 탐색기**개방형 `MainActivity.cs` 기존 코드를 다음 코드로 바꿉니다.

    ```csharp
        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }
    ```

    대체 `<insert_WCF_service_endpoint_here>` WCF 끝점의 주소를 사용 합니다.

1. `MainActivity.cs`, 수정는 `OnCreate` 메서드에 다음 코드 포함 되도록 합니다.

    ```csharp
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }
    ```
    
    위의 코드는 클래스에 대 한 인스턴스 변수를 초기화 하 고 몇 가지 이벤트 처리기를 연결 합니다.

1. `MainActivity.cs`, 다음 두 가지 방법 추가 하 여 클라이언트 프록시 클래스를 인스턴스화합니다.

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
    
    위의 코드를 인스턴스화하고 초기화는 `HelloWorldServiceClient` 개체입니다.

1. `MainActivity.cs`, 짝수 처리기에 두 단추에 대 한 추가 `Activity`:

    ```csharp
        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```
  
1. 응용 프로그램을 실행 하 고 WCF 서비스가 실행 되 고 두 개의 단추 클릭을 확인 합니다. 응용 프로그램은 WCF를 비동기적으로 호출 하는 `Endpoint` 필드 올바르게 설정 되지 않습니다.

    [![](walkthrough-working-with-wcf-images/image08.png "30 초 내에서 각 WCF 메서드 응답이 수신 하 고 응용 프로그램은이 스크린샷 같은 화면이 표시")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Xamarin.iOS 응용 프로그램 만들기

WCF 서비스 프록시 Xamarin.iOS 응용 프로그램에 의해 다음과 같이 사용 될 수 있습니다.

1. Visual Studio에서 새 iPhone 추가 **단일 보기 응용 프로그램** 프로젝트를 솔루션 하 고 이름을 `HelloWorld.iOS`합니다.
1. 에 `HelloWorld.iOS` 프로젝트에서에 대 한 참조를 추가 `HelloWorldServiceProxy` 프로젝트 및에 대 한 참조는 `System.ServiceModel` 네임 스페이스입니다.
1. **솔루션 탐색기**를 두 번 클릭 `Main.storyboard` iOS 디자이너에서의 파일을 엽니다. 그런 다음에 다음 추가 `UIButton` 및 `UITextView` 컨트롤:

    ||이름|제목|
    |--- |--- |--- |
    |`UIButton`|`sayHelloWorldButton`|"Hello, World"를 표시 합니다.|
    |`UITextView`|`sayHelloWorldText`||
    |`UIButton`|`getHelloWorldDataButton`|"Hello, World" get 데이터|
    |`UITextView`|`getHelloWorldDataText`||

    컨트롤을 추가한 후 UI의 다음 스크린 샷에서 유사 합니다.

    [![](walkthrough-working-with-wcf-images/image12.png "UI는 컨트롤을 추가한 후이 스크린샷과 유사 합니다.")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. **솔루션 탐색기**열기, `ViewController.cs` 다음 코드를 추가 합니다.

    ```xml
        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }
    ```
  
    대체 `<insert_WCF_service_endpoint_here>` WCF 끝점의 주소를 사용 합니다.

1. `ViewController.cs`, 업데이트 된 `ViewDidLoad` 한다는 다음과 유사한 메서드:

    ```csharp
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }
    ```
  
1. `ViewController.cs`, 추가 된 `InitializeHelloWorldServiceClient` 및 `CreateBasicHttpBinding` 메서드:

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
  
1. `ViewController.cs`, 이벤트 처리기에 대 한 추가 `TouchUpInside` 두 이벤트 `UIButton` 인스턴스:

    ```csharp
        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```

1. 응용 프로그램을 실행 하 고 WCF 서비스가 실행 되 고 두 개의 단추 클릭을 확인 합니다. 응용 프로그램은 WCF를 비동기적으로 호출 하는 `Endpoint` 필드 올바르게 설정 되지 않습니다.

    [![](walkthrough-working-with-wcf-images/image10.png "30 초 내에서 각 WCF 메서드 응답이 수신 하 고 응용 프로그램은이 스크린샷과 같이 표시 됩니다.")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>요약

이 자습서는 Xamarin.iOS 및 Xamarin.Android를 사용 하 여 모바일 응용 프로그램에서 WCF 서비스 사용 방법을 설명 합니다. WCF 서비스를 만드는 방법을 보여 주었습니다 하 고 원격 장치에서 연결을 허용 하도록 Windows 10 및 IIS Express를 구성 하는 방법을 설명 합니다. 그런 다음 WCF 클라이언트 프록시를 생성 하는 방법을 설명 하 고 모드 해제 프록시 클라이언트 Xamarin.iOS 및 Xamarin.Android 응용 프로그램에서 사용 하는 방법을 보여 줍니다.


## <a name="related-links"></a>관련 링크

- [HelloWorld (샘플)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [Wcf 서비스 지향 응용 프로그램 개발](https://docs.microsoft.com/dotnet/framework/wcf/index)
- [방법: Windows Communication Foundation 클라이언트 만들기](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel Metadata 유틸리티 도구 (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
