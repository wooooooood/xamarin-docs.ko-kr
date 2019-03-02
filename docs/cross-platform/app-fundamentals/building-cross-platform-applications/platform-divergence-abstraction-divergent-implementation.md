---
title: 4부 - 다중 플랫폼 처리
description: 이 문서에서는 플랫폼 또는 기능 기반 응용 프로그램 확산을 처리 하는 방법을 설명 합니다. 화면 크기, 탐색 메타포, 터치 및 제스처, 푸시 알림 및 목록 탭 등 인터페이스 패러다임에 설명 합니다.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 8fb373aa5081c8937da5110bad47c3f0decf0126
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57197240"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>4부 - 다중 플랫폼 처리

## <a name="handling-platform-divergence-amp-features"></a>플랫폼 차이 처리 &amp; 기능

확산 되지 ' 플랫폼 ' 문제만. '같은' 플랫폼에는 장치에는 다양 한 기능 (특히 다양 한 사용할 수 있는 Android 장치)에 있습니다. 가장 분명 하 고 기본 화면 크기, 이지만 다른 장치 특성이 다르며 특정 기능을 확인 하 고 해당 (유무)에 따라 다르게 동작 하도록 응용 프로그램이 필요한 수 있습니다.

즉, 모든 응용 프로그램 보기 좋지 않은, 가장 낮은 공통 분모 기능 집합을 제공 하는 것이 또는 기능의 정상적인 저하를 사용 하 여 처리 해야 합니다. Xamarin의 긴밀 한 통합 네이티브 Sdk 사용 하 여 각 플랫폼의 이러한 기능을 사용 하는 앱을 디자인 하는 데 적합 하므로 플랫폼 특정 기능을 활용 하려면 응용 프로그램을 허용 합니다.

플랫폼 기능에 어떻게 다른 지에 대 한 개요 플랫폼 기능 설명서를 참조 합니다.

## <a name="examples-of-platform-divergence"></a>플랫폼 차이의 예

### <a name="fundamental-elements-that-exist-across-platforms"></a>플랫폼 간에 존재 하는 기본적인 요소

일부의 특성 유니버설 된 모바일 응용 프로그램입니다.
다음은 모든 장치에 일반적으로 응용 프로그램 디자인의 기초를 형성 하므로 수를 더 높은 수준의 개념입니다.

-  탭 또는 메뉴를 통해 기능 선택
-  목록 데이터 및 스크롤
-  데이터의 단일 보기
-  데이터의 단일 뷰를 편집합니다.
-  뒤로 탐색

고급 화면 흐름을 디자인할 때 이러한 개념에 공통 된 사용자 경험을 만들 수 있습니다.

### <a name="platform-specific-attributes"></a>플랫폼별 특성

모든 플랫폼에 존재 하는 기본 요소를 외에도 디자인의 주요 플랫폼 차이 해결 해야 합니다. 이러한 차이점을는 것이 좋습니다 (및 처리 하기 위해 코드를 작성) 해야 할 수 있습니다.

-   **화면 크기** – 일부 플랫폼 (예: iOS 및 Windows Phone 버전)가 대상으로 비교적 간단한 화면 크기를 표준화 합니다. Android 장치에 다양 한 화면 크기, 응용 프로그램에서 지 원하는 데 많은 노력이 필요 합니다.
-   **탐색 메타포** -(예: 플랫폼의 차이점 하드웨어 '뒤로' button, Panorama UI 컨트롤) 및 플랫폼 (Android 2 및 4, iPhone 및 iPad) 내에서.
-   **키보드** – 일부 Android 장치의 경우에 소프트웨어 키보드 중 실제 키보드. 소프트 키보드가 화면의 가리는 경우를 검색 하는 코드는 이러한 차이 있어야 합니다.
-   **터치 및 제스처** – 특히 이전 버전의 각 운영 체제에서에서 제스처 인식에 대 한 운영 체제 지원 달라 집니다. 이전 버전의 Android가 매우 제한적으로 지원 touch 작업의 경우 이전 버전의 장치를 지 원하는 별도 코드를 필요할 수 있음을 의미 합니다.
-   **푸시 알림** – 각 플랫폼 (예:에 다양 한 기능/구현 라이브 타일에 Windows).

### <a name="device-specific-features"></a>장치 관련 기능

확인할 응용 프로그램에 필요한 최소 기능 해야 합니다. 또는 경우 활용 하기 위해 각 플랫폼에서 추가 기능을 결정 합니다. 코드 검색 기능 및 기능을 사용 하지 않도록 설정 하거나 (예:에 대 한 대안을 제공 해야 지리적 위치 하는 대신 사용자가 위치를 입력 하거나 맵에서 선택 수 있음):

-   **카메라** – 장치에서 기능 다릅니다: 일부 장치에 카메라가 없는, 다른 사용자가 모두 전면 및 후면 카메라입니다. 일부 카메라 비디오 기록을 수 있습니다.
-   **지리적 위치 및 맵** – 모든 장치에서 Wi-fi 또는 GPS 위치에 대 한 지원이 없는 합니다. 앱은 또한 다양 한 수준의 각 메서드에서 지원 되는 정확도 제공 해야 합니다.
-   **가 속도계, 자이로스코프가 및 compass** – 앱 거의 항상 제공 해야는 대체 (fallback) 하드웨어 지원 되지 않습니다 있도록 선택한 각 플랫폼에서 장치에 이러한 기능은 흔히 발견 됩니다.
-   **Twitter 및 Facebook** iOS5 iOS6에만 '기본' – 각각. 사용자 고유의 인증 기능을 제공 하 고 각와 직접 인터페이스 하 해야 이전 버전 및 기타 플랫폼에서 서비스의 API입니다.
-   **필드 통신 (NFC) 거의** – (일부)에 대해서만 Android 휴대폰 (작성 시).

## <a name="dealing-with-platform-divergence"></a>플랫폼 차이 사용 하 여 처리

동일한 코드 베이스를 각각 장점과 단점 자체 집합에서 여러 플랫폼을 지원 하기 위해 두 가지 방법이 있습니다.

-   **플랫폼 추상화** – 비즈니스 외관 패턴 플랫폼에서 통합 된 액세스를 제공 하 고 통합 된 단일 API로 플랫폼 특정 구현을 추상화 합니다.
-   **분기 구현** – 인터페이스 및 상속 또는 조건부 컴파일 등 아키텍처 도구를 통해 분기 구현을 통해 기능을 특정 플랫폼을 호출 합니다.

## <a name="platform-abstraction"></a>플랫폼 추상화

### <a name="class-abstraction"></a>클래스 추상화

인터페이스 또는 기본 클래스를 사용 하 여 공유 코드에 정의 되 고 구현 또는 플랫폼별 프로젝트에 확장. 작성 하 고 확장 클래스 추상화를 사용 하 여 공유 코드는 프레임 워크를 사용할 수의 제한 된 하위 집합을가지고 있으며 플랫폼별 코드 분기를 지원 하기 위해 컴파일러 지시문을 포함할 수 없습니다 때문에 이식 가능한 클래스 라이브러리에 특히 적합 합니다.

#### <a name="interfaces"></a>인터페이스

인터페이스를 사용 하 여 공통 코드를 활용 하 여 공유 라이브러리를 계속 전달할 수 있는 플랫폼 특정 클래스를 구현할 수 있습니다.

인터페이스는 공유 코드에서 정의 되 고 매개 변수 또는 속성으로 공유 라이브러리로 전달 합니다.

플랫폼 관련 응용 프로그램은 다음 인터페이스를 구현 하 고 '처리' 하는 공유 코드 활용 하기 위해 여전히 수 있습니다.

 **장점**

구현은은 플랫폼별 코드를 포함 하 고도 플랫폼별 외부 라이브러리를 참조할 수 있습니다.

 **단점**

만들고 구현을 공유 코드에 전달할 필요 합니다. 공유 코드 내에서 심층 인터페이스를 사용 하는 경우 다음 종료 되 여러 메서드 매개 변수를 통해 전달 되거나 그렇지 않으면 호출 체인을 통해 푸시 다운 합니다. 공유 코드에는 다른 인터페이스를 많이 사용 하는 경우 다음 이러한 모든를 만든 다음 공유 코드 위치에서 설정 합니다.

#### <a name="inheritance"></a>상속

공유 코드를 하나 이상의 플랫폼 특정 프로젝트에서 확장 될 수 있습니다. 있는 추상 또는 가상 클래스를 구현할 수 있습니다. 이 비슷한 인터페이스를 사용 하 여 하지만 이미 구현 하는 몇 가지 동작. 에 다양 한 뷰포인트 인터페이스 또는 상속 되는지 더 나은 디자인을 선택 합니다: 특히 C#에서는 단일 상속 하기 때문에 수 지정 서 앞으로 Api를 디자인할 수 있습니다. 주의 해 서 상속을 사용 합니다.

장점 및 단점 인터페이스의 기본 클래스 구현 코드가 (아마도 전체 플랫폼 독립적인 구현 필요에 따라 확장할 수 있는)를 포함할 수 있는 추가적인 이점도 사용 하 여 상속에 동일 하 게 적용 됩니다.

## <a name="xamarinforms"></a>Xamarin.Forms

참조 된 [Xamarin.Forms](~/get-started/index.yml) 설명서.

### <a name="other-cross-platform-libraries"></a>다른 플랫폼 간 라이브러리

이러한 라이브러리는 또한에 대 한 플랫폼 간 기능을 제공 C# 개발자:

- [**Xamarin.Essentials** ](~/essentials/index.md) – 일반적인 기능에 대 한 플랫폼 간 Api.
- [**SkiaSharp** ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) -플랫폼 간 2D 그래픽입니다.

## <a name="conditional-compilation"></a>조건부 컴파일

공유 코드는 클래스 또는 다르게 작동 하는 기능에 액세스할 수 있는 각 플랫폼에서 다르게 작동 하도록 해야 하는 경우가 있습니다. 조건부 컴파일 같은 소스 파일에 정의 된 다른 기호를 포함 하는 여러 프로젝트 참조 하 고 있는 공유 자산 프로젝트를 사용 하 여 가장 적합 합니다.

Xamarin 프로젝트를 정의 하는 `__MOBILE__` 는 iOS 및 Android 응용 프로그램 프로젝트 (사전 및 사후 수정 이러한 기호에 두 개의 밑줄 참고) 모두에 마찬가지입니다.

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```
#### <a name="ios"></a>iOS

Xamarin.iOS 정의 `__IOS__` iOS 장치를 검색 하는 데 사용할 수 있는 합니다.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

조사식 및 TV 별 기호는:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

#### <a name="android"></a>Android

Xamarin.Android 응용 프로그램에 컴파일만 해야 하는 코드를 다음 사용할 수 있습니다.

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

각 API 버전도 새 컴파일러 지시문을 정의 새 Api를 대상으로 하는 경우 기능을 추가 하 다음과 같은 코드 수 있도록 합니다. 각 API 수준은 '아래쪽' 모든 수준을 기호를 포함합니다. 이 기능은 여러 플랫폼; 지원에 실제로 유용 하지 않습니다. 일반적으로 `__ANDROID__` 기호 만으로도 충분 합니다.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

#### <a name="mac"></a>Mac

없는 현재 Xamarin.Mac에 대 한 기본 제공 기호 있지만 Mac에서 직접 앱 프로젝트를 추가할 수 있습니다 **옵션 > 빌드 > 컴파일러** 에 **기호를 정의할** 하거나 편집 합니다 **.csproj**  파일을 추가할 수 있습니다 (예를 들어 `__MAC__`)

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

#### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

`WINDOWS_UWP`을 사용하세요. Xamarin 플랫폼 기호 문자열이 주변에 밑줄이 있습니다.

```csharp
#if WINDOWS_UWP
// UWP-specific code
#endif
```

#### <a name="using-conditional-compilation"></a>조건부 컴파일을 사용

조건부 컴파일 예제 사례 연구는 SQLite 데이터베이스 파일의 파일 위치를 설정 합니다. 세 가지 플랫폼에는 파일 위치를 지정 하는 데 약간 다른 요구 사항이 있습니다.

-   **iOS** – Apple에서 사용자가 아닌 데이터를 특정 위치 (라이브러리 디렉터리)에 저장할 수 있지만이 디렉터리에 대 한 시스템 상수가 없는 합니다. 플랫폼 특정 코드는 올바른 경로 작성 해야 합니다.
-   **Android** – 반환 되는 시스템 경로의 `Environment.SpecialFolder.Personal` 데이터베이스 파일을 저장할 허용 되는 위치입니다.
-   **Windows Phone** – 격리 된 저장소 메커니즘의 전체 경로를 지정할 수 없도록만 상대 경로 및 파일 이름입니다.
-   **유니버설 Windows 플랫폼** – 사용 `Windows.Storage` Api.

다음 코드는 조건부 컴파일을 사용 하 여 되도록는 `DatabaseFilePath` 각 플랫폼에 대 한 올바른:

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

결과 작성 하 고 각 플랫폼에서 다른 위치에 SQLite 데이터베이스 파일을 배치 하는 모든 플랫폼에서 사용할 수 있는 클래스가 생성 됩니다.
