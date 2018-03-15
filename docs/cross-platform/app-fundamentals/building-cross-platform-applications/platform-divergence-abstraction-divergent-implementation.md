---
title: "4 부-여러 플랫폼을 다루는"
ms.topic: article
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 21cd08ad2eb9c78ba1bcd6b31400a38266c65e51
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="part-4---dealing-with-multiple-platforms"></a>4 부-여러 플랫폼을 다루는

## <a name="handling-platform-divergence-amp-features"></a>플랫폼 확산 처리 &amp; 기능

분기는 ' 플랫폼 ' 문제가; 방금 되지 않습니다. '동일한' 플랫폼의 장치는 서로 다른 기능이 (특히는 광범위 한 사용할 수 있는 Android 장치). 가장 확실 하 고 기본 화면 크기 않으며 다른 장치 특성 다르며 들은 특정 기능을 확인 하 고 자신의 (유무)에 따라 다르게 동작 하도록 응용 프로그램 수입니다.

즉, 모든 응용 프로그램의 기능 정상적인 저하를 다루는. 그렇지 않으면 이상한, 최저 공통 분모 기능 집합을 제공 해야 합니다. Xamarin의 긴밀 한 통합으로 네이티브 Sdk는 각 플랫폼의 이러한 기능을 사용 하는 앱을 디자인 하는 의미가 있으므로 플랫폼 특정 기능을 활용 하도록 응용 프로그램을 허용 합니다.

플랫폼 기능이 어떻게 다른 지에 대 한 개요는 플랫폼 기능 설명서를 참조 하십시오.

 <a name="Examples_of_Platform_Divergence" />


### <a name="examples-of-platform-divergence"></a>플랫폼 차이의 예

 <a name="Fundamental_elements_that_exist_across_platforms" />


#### <a name="fundamental-elements-that-exist-across-platforms"></a>플랫폼 간에 존재 하는 기본 요소

유니버설 있는 모바일 응용 프로그램의 특징도 있습니다.
모든 장치에 일반적으로 해당 응용 프로그램의 디자인의 기초를 형성 하므로 수 있으며 더 높은 수준의 개념은 다음과 같습니다.

-  메뉴 또는 탭을 통해 기능 선택
-  데이터 및 스크롤 목록이
-  데이터의 단일 뷰
-  데이터의 단일 뷰를 편집합니다.
-  뒤로 탐색


높은 수준의 화면 흐름을 디자인할 때 이러한 개념에 공통 된 사용자 경험을 만들 수 있습니다.

 <a name="platform-specific_attributes" />


#### <a name="platform-specific-attributes"></a>플랫폼 관련 특성

모든 플랫폼에 존재 하는 기본 요소를 외에 디자인에 주소 키 플랫폼의 차이점을 해야 합니다. 이러한 차이 것이 좋습니다 (및 처리 하기 위해 코드를 작성) 해야 할 수 있습니다.

-   **화면 크기** – 일부 플랫폼 (예: iOS 및 Windows Phone 이전 버전)은 비교적 간단 대상으로 한 화면 크기 표준화 합니다. Android 장치의 경우 다양 한 화면 크기는 응용 프로그램에서 지 원하는 데 많은 노력이 필요 합니다.
-   **탐색 메타포** -(예: 플랫폼의 차이점. 하드웨어 '뒷면' 단추를 UI 파노라마 컨트롤) 및 플랫폼 (Android 2 및 4, iPhone 및 iPad) 내에서.
-   **키보드** – 일부 Android 장치의 경우에 소프트웨어 키보드를 했지만 다른 물리적 키보드 합니다. 소프트 키보드는 화면 부분에 표시 되지 않도록 하는 경우를 검색 하는 코드는 이러한 차이에 민감한 있어야 합니다.
-   **터치 및 제스처** – 각 운영 체제의 이전 버전에서 특히 제스처를 인식 운영 체제 지원은 달라 집니다. Android의 이전 버전에는 매우 제한적으로 오래 된 장치를 지 원하는 별도 코드를 필요할 수 있음을 의미 하는 터치 작업에 대 한 지원
-   **푸시 알림** – 않음 (예: 각 플랫폼에는 서로 다른 기능/구현 라이브 타일 Windows에서).


 <a name="Device-specific_features" />


#### <a name="device-specific-features"></a>장치 전용 기능

응용 프로그램에 필요한 최소 기능 해야 할; 결정 또는 때 활용 하는 각 플랫폼에서 추가 기능을 결정 합니다. 코드를 검색 하 고 기능 및 기능을 사용 하지 않음 (예: 대안을 제공 해야 합니다. 지리적 위치 하는 대신 사용자 위치를 입력 하거나 지도에서 선택할 수 있음):

-   **카메라** – 기능이 다를 장치 간에: 일부 장치 카메라를 다른 사용자는 모두 전면 및 후면 연결 카메라가 되지 합니다. 일부 카메라 비디오 기록을 수 있습니다.
-   **지리적 위치 및 맵** – GPS 또는 Wi-fi 위치에 대 한 지원 모든 장치에 나타나지 않습니다. 또한 응용 프로그램에 대 한 다양 한 수준의 각 방법에서 지원 되는 정확도 충족 해야 합니다.
-   **가 속도계, 자이로스코프 및 나침반** – 되므로 앱 하드웨어를 지원 하지 않을 때 한 대체를 제공할 필요가 거의 항상 선택한 각 플랫폼에는 장치에서 이러한 기능 자주 발견 됩니다.
-   **Facebook 및 twitter** iOS5 및 iOS6에만 '기본' – 각각. 이전 버전 및 기타 플랫폼에서 할 사용자 고유의 인증 기능을 제공 하 고 각와 직접 상호 작용할 서비스의 API입니다.
-   **통신 NFC (근거리) 근처** – (일부)에 대해서만 Android phone (작성 시)입니다.


 <a name="Dealing_with_Platform_Divergence" />


### <a name="dealing-with-platform-divergence"></a>플랫폼 확산 처리

에 동일한 코드 베이스, 이점 및 단점이의 자체 집합을 가진에서 여러 플랫폼을 지 원하는 두 가지 방법이 있습니다.

-   **플랫폼 추상화** – 비즈니스 외관 패턴 플랫폼 간에 통합 된 액세스를 제공 하 고 특정 플랫폼 구현을 통해 통합 된 단일 API 추상화 합니다.
-   **분기 구현** – 인터페이스 및 상속 또는 조건부 컴파일 등 아키텍처 도구를 통해 다른 구현을 통해 기능이 특정 플랫폼의 호출 합니다.


 <a name="Platform_Abstraction" />


## <a name="platform-abstraction"></a>플랫폼 추상화

 <a name="Class_Abstraction" />


### <a name="class-abstraction"></a>추상 클래스

인터페이스 또는 기본 클래스 중 하나를 사용 하 여 공유 코드에 정의 되 고 구현 되거나 플랫폼별 프로젝트에 따라 확장 합니다. 작성 하 고 확장 클래스 추상화를 사용 하 여 공유 코드는 플랫폼별 코드 분기를 지원 하기 위해 컴파일러 지시문을 사용할 수 없습니다 및 제한 된 하위 집합을 사용할 수 있는 프레임 워크는 때문에 이식 가능한 클래스 라이브러리에 특히 적합 합니다.

 <a name="Interfaces" />


#### <a name="interfaces"></a>인터페이스

인터페이스를 사용 하 여 공통 코드를 활용 하 여 공유 라이브러리에 여전히 전달 될 수 있는 플랫폼 관련 클래스를 구현할 수 있습니다.

인터페이스는 공유 코드에 정의 되어 있으며 공유 라이브러리에 매개 변수 또는 속성으로 전달 합니다.

플랫폼 특정 응용 프로그램 다음 인터페이스를 구현 하 고 공유 코드 '처리'를 계속 사용 합니다.

 **장점**

구현도 플랫폼별 외부 라이브러리를 참조 및 플랫폼별 코드를 포함할 수 있는 합니다.

 **단점**

만들고 구현 공유 코드에 전달할 필요 합니다. 인터페이스는 공유 코드 내 심층 사용 하는 경우 다음을 종료 되 고 그렇지 않으면 호출 체인을 통해 적용할지 또는 여러 메서드 매개 변수를 통해 전달 합니다. 공유 코드에는 서로 다른 인터페이스를 많이 사용 하는 경우 다음은 모두을 만들고 공유 코드를 다른 위치에서 설정 합니다.

 <a name="Inheritance" />


#### <a name="inheritance"></a>상속

공유 코드 하나 이상의 플랫폼별 프로젝트에 확장 될 수 있는 가상 또는 추상 클래스를 구현할 수 있습니다. 이 비슷한 인터페이스를 사용 하 여 이미 구현 된 몇 가지 동작을 사용 하면서 합니다. 인터페이스 또는 상속 되는지 더 나은 디자인 선택에는 다양 한 뷰포인트: 특히 C#만 허용 단일 상속 하기 때문에 그 수 사항이 앞으로 Api를 디자인할 수 있는 방법입니다. 주의 해 서 상속을 사용 합니다.

장점 및 단점 인터페이스의 상속 된 기본 클래스 구현 (아마도 전체 플랫폼 중립적 구현 필요에 따라 확장 될 수 있는) 코드를 포함할 수 있는 추가적인 이점도에 동일 하 게 적용 됩니다.

<a name="Xamarin.Forms" />

### <a name="xamarinforms"></a>Xamarin.Forms

참조는 [Xamarin.Forms](~/xamarin-forms/get-started/index.md) 설명서입니다.


### <a name="plug-in-cross-platform-functionality"></a>플러그 인 플랫폼 간 기능

플러그 인을 사용 하 여 일관 된 방식으로 플랫폼 간 앱을 확장할 수도 있습니다.

연결 된 우리의 [플러그 인 github](https://github.com/xamarin/plugins), 대부분 플러그 인은 오픈 소스 프로젝트 (일반적으로 사용할 수 있는 Nuget 통해 설치) 하는 데 도움이 되는 다양 한 설정 배터리 상태에서 플랫폼 관련 기능을 구현할는 Xamarin 플랫폼 및 Xamarin.Forms 앱에서 사용 하기 쉬운 일반적인 API입니다.


<a name="Other_Cross-Platform_Libraries" />

### <a name="other-cross-platform-libraries"></a>다른 플랫폼 간 라이브러리

사용할 수 있는 플랫폼 간 기능을 제공 하는 타사 라이브러리의 여러 가지:

-   **MvvmCross** -  [https://github.com/slodge/MvvmCross/](https://github.com/slodge/MvvmCross/)
-   **기업** (지역화를 위해)-  [https://github.com/rdio/vernacular/](https://github.com/rdio/vernacular/)
-   **MonoGame** (XNA 게임)-에 대 한  [http://monogame.codeplex.com/](http://monogame.codeplex.com/)
-   **NGraphics** - [NGraphics](https://github.com/praeclarum/NGraphics) 및 해당 앞서 수행 [https://github.com/praeclarum/CrossGraphics](https://github.com/praeclarum/CrossGraphics)


 <a name="Divergent_Implementation" />


### <a name="divergent-implementation"></a>분기 구현

 <a name="Conditional_Compilation" />


#### <a name="conditional-compilation"></a>조건부 컴파일

공유 코드는 클래스 또는 다르게 동작 하는 기능에 액세스할 수 있는 각 플랫폼에서 다르게 작동 하도록 해야 하는 경우가 있습니다. 조건부 컴파일 공유 자산 프로젝트의 경우 서로 다른 기호는 정의 된 여러 프로젝트에서 참조 하 고 동일한 소스 파일 위치와 가장 적합 합니다.

Xamarin 프로젝트를 항상 정의 `__MOBILE__` 는 iOS 및 Android 응용 프로그램 프로젝트 (전처리 및 후 수정 이러한 기호에 두 개의 밑줄 참고)에 마찬가지입니다.

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

<a name="iOS" />

##### <a name="ios"></a>iOS

Xamarin.iOS 정의 `__IOS__` 는 iOS 장치를 검색 하는 데 사용할 수 있습니다.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

조사식 및 TV 관련 기호 있습니다.

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

<a name="Android" />

##### <a name="android"></a>Android

Xamarin.Android 응용 프로그램에만 컴파일되는 코드는 다음을 사용할 수 있습니다.

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

새 Api를 대상으로 하는 경우 기능을 추가 하면 다음과 같은 코드 수 있으므로, 각 API 버전도 새 컴파일러 지시문을 정의 합니다. 각 API 수준은 '아래쪽' 모든 수준 기호를 포함합니다. 이 기능은; 여러 플랫폼을 지 원하는에 실제로 유용 하지 않습니다. 일반적으로 `__ANDROID__` 기호 만으로도 충분 합니다.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

##### <a name="mac"></a>Mac

하지 현재 Xamarin.Mac에 대 한 기본 제공 기호는 않지만 Mac에서 직접 응용 프로그램 프로젝트를 추가할 수 있습니다 **옵션 > 빌드 > 컴파일러** 에 **기호를 정의** 하거나 편집는 **.csproj**  파일을 추가 있습니다 (예를 들어 `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

<a name="Windows_Phone" />

##### <a name="windows-phone"></a>Windows Phone

Windows Phone 앱에는 두 개의 기호 – 정의 `WINDOWS_PHONE` 및 `SILVERLIGHT` -플랫폼에 코드를 대상으로 사용할 수 있는 합니다. 이러한 않습니다 Xamarin 플랫폼 기호 처럼 주변에 밑줄 필요는 없습니다.


<a name="Using_Conditional_Compilation" />

##### <a name="using-conditional-compilation"></a>조건부 컴파일을 사용

조건부 컴파일의 간단한 사례 연구 예 SQLite 데이터베이스 파일에 대 한 파일 위치를 설정입니다. 세 플랫폼에는 파일 위치를 지정 하기 위한 약간 다른 요구 사항이 있습니다.

-   **iOS** – Apple 비 사용자 데이터 (라이브러리 디렉터리) 특정 위치에 배치를 선호 하지만이 디렉터리에 대 한 시스템 상수가 있습니다. 올바른 경로를 빌드하려면 플랫폼별 코드가 필요 합니다.
-   **Android** –에서 반환 되는 시스템 경로 `Environment.SpecialFolder.Personal` 데이터베이스 파일을 저장할 허용 되는 위치입니다.
-   **Windows Phone** – 격리 된 저장소 메커니즘의 전체 경로를 지정할 수 없도록 합니다. 상대 경로 파일 이름을 합니다.
-   **유니버설 Windows 플랫폼** – 사용 하 여 `Windows.Storage` Api입니다.

다음 코드는 조건부 컴파일을 사용 하 여 되도록는 `DatabaseFilePath` 은 각 플랫폼에 적합 합니다.

```csharp
public static string DatabaseFilePath {
        get {
    var filename = "TodoDatabase.db3";
#if SILVERLIGHT
    // Windows Phone 8
    var path = filename;
#else

#if __ANDROID__
    string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine (libraryPath, filename);
#endif
        return path;
}
```

결과 빌드 및 각 플랫폼에서 다른 위치에 SQLite 데이터베이스 파일을 배치 하는 모든 플랫폼에서 사용할 수 있는 클래스입니다.

