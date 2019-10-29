---
title: 4부 - 다중 플랫폼 처리
description: 이 문서에서는 플랫폼 또는 기능에 따라 응용 프로그램 확산을 처리 하는 방법을 설명 합니다. 화면 크기, 탐색 메타포, 터치/제스처, 푸시 알림 및 목록 및 탭과 같은 인터페이스 패러다임에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 555723e689a9ba076ee34d49b93cf7141e542832
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016885"
---
# <a name="part-4---dealing-with-multiple-platforms"></a>4부 - 다중 플랫폼 처리

## <a name="handling-platform-divergence-amp-features"></a>플랫폼 확산 &amp; 기능 처리

' 플랫폼 간 ' 문제는 아닙니다. ' 동일한 ' 플랫폼의 장치에는 서로 다른 기능이 있습니다 (특히 사용 가능한 다양 한 Android 장치). 가장 명확 하 고 기본적인 화면은 화면 크기 이지만 다른 장치 특성은 응용 프로그램에서 특정 기능을 확인 하 고 현재 상태 (또는 부재)에 따라 다르게 동작 하도록 요구 합니다.

즉, 모든 응용 프로그램은 기능의 정상 저하를 처리 하거나, 끌지, 가장 낮은 공통-분모 기능 집합을 제공 해야 합니다. 각 플랫폼의 네이티브 Sdk와 Xamarin의 긴밀 한 통합을 통해 응용 프로그램은 플랫폼별 기능을 활용할 수 있으므로 이러한 기능을 사용 하도록 앱을 설계 하는 것이 좋습니다.

플랫폼이 기능에서 어떻게 다른 지에 대 한 개요는 플랫폼 기능 설명서를 참조 하세요.

## <a name="examples-of-platform-divergence"></a>플랫폼의 확산 예

### <a name="fundamental-elements-that-exist-across-platforms"></a>여러 플랫폼에 존재 하는 기본 요소

범용 응용 프로그램에는 몇 가지 특징이 있습니다.
이러한 개념은 모든 장치에 일반적으로 적용 되는 더 높은 수준의 개념 이며, 따라서 응용 프로그램 디자인의 기반을 형성 합니다.

- 탭 또는 메뉴를 통한 기능 선택
- 데이터 목록 및 스크롤
- 단일 데이터 뷰
- 데이터의 단일 뷰 편집
- 뒤로 탐색

개략적인 화면 흐름을 디자인할 때 이러한 개념에 대 한 일반적인 사용자 환경을 기반으로 할 수 있습니다.

### <a name="platform-specific-attributes"></a>플랫폼별 특성

모든 플랫폼에 존재 하는 기본 요소 외에도 디자인에서 주요 플랫폼의 차이점을 해결 해야 합니다. 이러한 차이를 처리 하기 위해 특별히 코드를 작성 해야 할 수도 있습니다.

- **화면 크기** – 일부 플랫폼 (예: iOS 및 이전 Windows Phone 버전)은 비교적 간단 하 게 대상으로 하는 표준화 된 화면 크기를 가집니다. Android 장치에는 응용 프로그램에서 지원 하기 위해 더 많은 노력을 해야 하는 다양 한 화면 차원이 있습니다.
- **탐색 메타포** – 플랫폼 마다 다릅니다 (예: 하드웨어 ' 뒤로 ' 단추, 파노라마 UI 컨트롤) 및 플랫폼 내 (Android 2 및 4, iPhone vs iPad)
- **키보드** – 일부 Android 장치에는 실제 키보드가 있지만 다른 장치에는 소프트웨어 키보드만 있습니다. 소프트 키보드의 일부가 화면에 포함 되는 경우를 감지 하는 코드는 이러한 차이를 인식 해야 합니다.
- **터치 및 제스처** – 제스처 인식에 대 한 운영 체제 지원은 특히 각 운영 체제의 이전 버전에서 다릅니다. 이전 버전의 Android는 터치 작업을 제한적으로 지원 합니다. 즉, 이전 장치를 지원 하려면 별도의 코드가 필요할 수 있습니다.
- **푸시 알림** – 각 플랫폼에 다른 기능/구현이 있습니다 (예: Windows의 라이브 타일).

### <a name="device-specific-features"></a>장치 관련 기능

응용 프로그램에 필요한 최소 기능을 결정 합니다. 또는 각 플랫폼에서를 활용 하는 추가 기능을 결정 합니다. 기능을 검색 하 고 기능 또는 제안 대안 (예: 지리적 위치 대신 사용자가 위치를 입력 하거나 map에서 선택할 수 있습니다.

- **카메라** – 장치 마다 기능이 다릅니다. 일부 장치에는 카메라가 없고 다른 장치에는 전면 및 후면 카메라가 모두 있습니다. 일부 카메라는 비디오 기록이 가능 합니다.
- **지리적 위치 & 맵** -GPS 또는 wi-fi 위치에 대 한 지원이 모든 장치에 표시 되지 않습니다. 또한 앱은 각 메서드에서 지원 되는 다양 한 수준의 정확성을 지원 해야 합니다.
- **가 속도계, 자이로스코프가 및 나침반** – 이러한 기능은 각 플랫폼에서 선택할 수 있는 장치에만 있지만, 앱이 지원 되지 않는 경우 앱은 거의 항상 대체를 제공 해야 합니다.
- **Twitter 및 Facebook** – IOS5 및 iOS6의 ' 기본 제공 '만 해당 합니다. 이전 버전 및 기타 플랫폼에서는 각 서비스의 API와 직접 고유한 인증 기능 및 인터페이스를 제공 해야 합니다.
- **NFC (근거리 통신** )-(일부) Android 휴대폰 (쓰기 시간)에만 해당 됩니다.

## <a name="dealing-with-platform-divergence"></a>플랫폼 확산 처리

동일한 코드 베이스에서 여러 플랫폼을 지 원하는 방법에는 두 가지가 있으며, 각각 고유한 이점 및 단점 집합입니다.

- **플랫폼 추상화** – 비즈니스 외관 패턴은 플랫폼 간에 통합 된 액세스를 제공 하 고 특정 플랫폼 구현을 단일 통합 API로 추상화 합니다.
- 서로 다른 **구현** – 인터페이스, 상속 또는 조건부 컴파일과 같은 아키텍처 도구를 통해 구현을 통해 특정 플랫폼 기능을 호출 합니다.

## <a name="platform-abstraction"></a>플랫폼 추상화

### <a name="class-abstraction"></a>클래스 추상화

공유 코드에 정의 된 인터페이스 또는 기본 클래스 중 하나를 사용 하 여 플랫폼별 프로젝트에서 구현 하거나 확장 합니다. 클래스 추상화를 사용 하 여 공유 코드를 작성 하 고 확장 하는 것은 프레임 워크의 제한 된 하위 집합을 포함 하 고 플랫폼별 코드 분기를 지 원하는 컴파일러 지시문을 포함할 수 없기 때문에 이식 가능한 클래스 라이브러리에 특히 적합 합니다.

#### <a name="interfaces"></a>인터페이스

인터페이스를 사용 하면 공통 코드를 활용 하기 위해 여전히 공유 라이브러리에 전달할 수 있는 플랫폼별 클래스를 구현할 수 있습니다.

인터페이스는 공유 코드에 정의 되 고 매개 변수 또는 속성으로 공유 라이브러리에 전달 됩니다.

그런 다음 플랫폼별 응용 프로그램은 인터페이스를 구현 하 고 여전히 공유 코드를 ' process '로 활용할 수 있습니다.

 **장점**

구현에는 플랫폼별 코드가 포함 될 수 있으며 플랫폼별 외부 라이브러리도 참조할 수 있습니다.

 **단점**

구현을 만들어 공유 코드에 전달 해야 합니다. 인터페이스가 공유 코드 내에서 사용 되는 경우 여러 메서드 매개 변수를 통해 전달 되거나 호출 체인을 통해 푸시되 지 게 됩니다. 공유 코드에서 다 수의 인터페이스를 사용 하는 경우 공유 코드 어딘가에 모든 인터페이스를 만들고 설정 해야 합니다.

#### <a name="inheritance"></a>상속

공유 코드는 하나 이상의 플랫폼별 프로젝트에서 확장 될 수 있는 추상 또는 가상 클래스를 구현할 수 있습니다. 이는 인터페이스를 사용 하는 것과 비슷하지만 일부 동작은 이미 구현 되어 있습니다. 인터페이스 또는 상속이 더 나은 디자인 선택 인지에 대 한 다양 한 뷰포인트 있습니다. 특히 단일 C# 상속만을 허용 하므로 api를 앞으로 디자인할 수 있는 방법을 지시할 수 있습니다. 상속을 주의 해 서 사용 합니다.

인터페이스의 장점과 단점은 상속에 동일 하 게 적용 되며, 기본 클래스에는 일부 구현 코드 (선택적으로 확장 될 수 있는 플랫폼에 관계 없는 구현)가 포함 될 수 있다는 이점도 있습니다.

## <a name="xamarinforms"></a>Xamarin.Forms

[Xamarin.ios](~/get-started/index.yml) 설명서를 참조 하세요.

### <a name="other-cross-platform-libraries"></a>기타 플랫폼 간 라이브러리

또한 이러한 라이브러리는 개발자를 위한 플랫폼 간 C# 기능을 제공 합니다.

- [**Xamarin.ios**](~/essentials/index.md) – 일반적인 기능을 위한 플랫폼 간 api입니다.
- [**SkiaSharp**](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) – 플랫폼 간 2d 그래픽.

## <a name="conditional-compilation"></a>조건부 컴파일

공유 코드는 각 플랫폼에서 다르게 작동 해야 하는 경우도 있습니다. 이러한 경우 다르게 동작 하는 클래스 또는 기능에 액세스할 수 있습니다. 조건부 컴파일은 동일한 소스 파일이 다른 기호가 정의 된 여러 프로젝트에서 참조 되는 공유 자산 프로젝트에서 가장 잘 작동 합니다.

Xamarin 프로젝트는 항상 iOS 및 Android 응용 프로그램 프로젝트에 대해 true 인 `__MOBILE__`를 정의 합니다. 이러한 기호에 대 한 이중 밑줄 사전 및 사후 픽스를 확인 합니다.

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

#### <a name="ios"></a>iOS

Xamarin.ios는 iOS 장치를 검색 하는 데 사용할 수 있는 `__IOS__` 정의 합니다.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

또한 감시 및 TV 관련 기호가 있습니다.

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

#### <a name="android"></a>Android

Xamarin으로 컴파일해야 하는 코드입니다. Android 응용 프로그램은 다음을 사용할 수 있습니다.

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

또한 각 API 버전은 새 컴파일러 지시어를 정의 하므로, 최신 Api를 대상으로 하는 경우 기능을 추가할 수 있습니다. 각 API 수준에는 ' 낮은 ' 수준 기호가 모두 포함 됩니다. 이 기능은 여러 플랫폼을 지 원하는 데 그다지 유용 하지 않습니다. 일반적으로 `__ANDROID__` 기호 면 충분 합니다.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

#### <a name="mac"></a>Mac

현재 Xamarin.ios에 대 한 기본 제공 기호가 없지만, Mac 앱 프로젝트 옵션에서 사용자 고유의 기호를 추가 하거나, **기호 정의** 상자에서 **> 컴파일러 > 빌드** 를 추가 하거나, .csproj 파일을 편집 하 고 해당 파일을 추가할 수 있습니다 (예: `__MAC__`) **.**

```xml
<PropertyGroup><DefineConstants>__MAC__;$(DefineConstants)</DefineConstants></PropertyGroup>
```

#### <a name="universal-windows-platform-uwp"></a>UWP(Universal Windows Platform)

`WINDOWS_UWP`을 사용하세요. Xamarin 플랫폼 기호와 같은 문자열 주위에는 밑줄이 없습니다.

```csharp
#if WINDOWS_UWP
// UWP-specific code
#endif
```

#### <a name="using-conditional-compilation"></a>조건부 컴파일 사용

조건부 컴파일의 간단한 사례 연구 예는 SQLite 데이터베이스 파일에 대 한 파일 위치를 설정 하는 것입니다. 세 플랫폼은 파일 위치를 지정 하기 위한 요구 사항이 약간 다릅니다.

- **iOS** – Apple은 사용자가 지정 하지 않은 데이터를 특정 위치 (라이브러리 디렉터리)에 배치 하도록 선호 하지만이 디렉터리에 대 한 시스템 상수는 없습니다. 올바른 경로를 빌드하려면 플랫폼별 코드가 필요 합니다.
- **Android** – `Environment.SpecialFolder.Personal`에서 반환 된 시스템 경로는 데이터베이스 파일을 저장 하는 데 사용할 수 있는 위치입니다.
- **Windows Phone** – 격리 된 저장소 메커니즘에서 전체 경로를 지정 하는 것은 허용 되지 않습니다. 상대 경로 및 파일 이름입니다.
- **유니버설 Windows 플랫폼** – `Windows.Storage` api를 사용 합니다.

다음 코드에서는 조건부 컴파일을 사용 하 여 각 플랫폼에 대 한 `DatabaseFilePath` 올바른지 확인 합니다.

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

결과는 모든 플랫폼에서 작성 및 사용 될 수 있는 클래스입니다. SQLite 데이터베이스 파일은 각 플랫폼의 다른 위치에 배치 합니다.
