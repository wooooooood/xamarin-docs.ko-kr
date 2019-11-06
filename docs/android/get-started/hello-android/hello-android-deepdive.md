---
title: 'Hello, Android: 심층 분석'
description: 두 부분으로 구성된 이 가이드에서는 첫 번째 Xamarin.Android 애플리케이션을 빌드하고 Xamarin을 사용하여 Android 애플리케이션 개발에 대한 기본 사항을 이해하기 시작합니다. 이 과정에서 Xamarin.Android 애플리케이션을 빌드하고 배포하는 데 필요한 도구, 개념 및 단계를 소개합니다.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: EF0E110B-20EA-43F6-9476-1A0F41AFD298
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: ee72c51611503f92e7ede3a01a7918780652935c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028003"
---
# <a name="hello-android-deep-dive"></a>Hello, Android: 심층 분석

_두 부분으로 구성된 이 가이드에서는 첫 번째 Xamarin.Android 애플리케이션을 빌드하고 Xamarin을 사용하여 Android 애플리케이션 개발에 대한 기본 사항을 이해하기 시작합니다. 이 과정에서 Xamarin.Android 애플리케이션을 빌드하고 배포하는 데 필요한 도구, 개념 및 단계를 소개합니다._

[Hello, Android 빠른 시작](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md)에서 첫 번째 Xamarin.Android 애플리케이션을 빌드하고 실행했습니다. 이제, 더 복잡한 프로그램을 빌드할 수 있도록 Android 애플리케이션의 작동 방식을 심층적으로 알아볼 시간입니다. 이 가이드에서는 Android 애플리케이션 개발에 대한 작업을 이해하고 기본 사항을 이해하기 시작할 수 있도록 Hello, Android 연습에서 수행한 단계를 검토합니다.

이 가이드는 다음 항목을 다룹니다.

::: zone pivot="windows"

- **Visual Studio 소개**&ndash; Visual Studio 및 새로운 Xamarin.Android 애플리케이션 만들기를 소개합니다.

- **Xamarin.Android 애플리케이션 분석** - Xamarin.Android 애플리케이션의 핵심 부분을 안내합니다.

- **앱 기본 항목 및 아키텍처 기본 사항** &ndash; 작업, Android 매니페스트 및 일반 버전의 Android 개발을 소개합니다.

- **UI(사용자 인터페이스)** &ndash; Android Designer를 통해 사용자 인터페이스를 만듭니다.

- **작업 및 작업 수명 주기** &ndash; 작업 수명 주기를 소개하고 코드로 사용자 인터페이스를 연결합니다.

- **테스트, 배포 및 마무리**&ndash; 테스트, 배포, 아트워크 생성 등에 관한 정보를 활용하여 애플리케이션을 완성합니다.

::: zone-end
::: zone pivot="macos"

- **Mac용 Visual Studio 소개**&ndash; Mac용 Visual Studio 및 새로운 Xamarin.Forms 애플리케이션 만들기를 소개합니다.

- **Xamarin.Android 애플리케이션 분석**&ndash; Xamarin.Android 애플리케이션의 핵심 부분을 안내합니다.

- **앱 기본 항목 및 아키텍처 기본 사항** &ndash; 작업, Android 매니페스트 및 일반 버전의 Android 개발을 소개합니다.

- **UI(사용자 인터페이스)** &ndash; Android Designer를 통해 사용자 인터페이스를 만듭니다.

- **작업 및 작업 수명 주기** &ndash; 작업 수명 주기를 소개하고 코드로 사용자 인터페이스를 연결합니다.

- **테스트, 배포 및 마무리**&ndash; 테스트, 배포, 아트워크 생성 등에 관한 정보를 활용하여 애플리케이션을 완성합니다.

::: zone-end

이 가이드는 단일 화면 Android 애플리케이션을 빌드하는 데 필요한 기술 및 정보를 개발하는 데 유용합니다. 이를 통해 Xamarin.Android 애플리케이션의 서로 다른 파트를 이해하고 서로 조정하는 방법에 대해 이해해야 합니다.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Visual Studio 소개

Visual Studio는 Microsoft의 강력한 IDE입니다. 완전히 통합된 비주얼 디자이너, 리팩터리 도구가 포함된 텍스트 편집기, 어셈블리 브라우저, 소스 코드 통합 등을 제공합니다. 이 가이드에서는 Xamarin 플러그 인을 사용하는 일부 기본 Visual Studio 기능을 사용하는 방법을 알아봅니다.

Visual Studio는 코드를 _솔루션_ 및 _프로젝트_로 구성합니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 애플리케이션(예: iOS 또는 Android용), 지원 라이브러리, 테스트 애플리케이션 등이 될 수 있습니다. **Phoneword** 앱에서 **Android 애플리케이션** 템플릿을 사용하는 새 Android 프로젝트를 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 가이드에서 만든 **Phoneword** 솔루션에 추가했습니다.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Mac용 Visual Studio 소개

Mac용 Visual Studio는 Visual Studio와 유사한 무료 오픈 소스 IDE입니다. 완전히 통합된 비주얼 디자이너, 리팩터리 도구가 함께 포함된 텍스트 편집기, 어셈블리 브라우저, 소스 코드 통합 등을 제공합니다. 이 가이드에서는 일부 기본 Mac용 Visual Studio 기능을 사용하는 방법을 알아봅니다. Mac용 Visual Studio를 처음 사용하는 경우 [Mac용 Visual Studio 소개](https://docs.microsoft.com/visualstudio/mac/)에 대해 자세히 확인하는 것이 좋습니다.

Mac용 Visual Studio는 코드를 _솔루션_ 및 _프로젝트_로 구성하는 Visual Studio 연습을 따릅니다. 솔루션은 하나 이상의 프로젝트를 포함할 수 있는 컨테이너입니다. 프로젝트는 애플리케이션(예: iOS 또는 Android용), 지원 라이브러리, 테스트 애플리케이션 등이 될 수 있습니다. **Phoneword** 앱에서 **Android 애플리케이션** 템플릿을 사용하는 새 Android 프로젝트를 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 가이드에서 만든 **Phoneword** 솔루션에 추가했습니다.

::: zone-end

<a name="anatomy" />

## <a name="anatomy-of-a-xamarinandroid-application"></a>Xamarin.Android 애플리케이션 분석

::: zone pivot="windows"

다음 스크린샷에서는 솔루션의 콘텐츠를 나열합니다. 솔루션 탐색기에는 솔루션과 연결된 디렉터리 구조와 모든 파일이 포함됩니다.

[![솔루션 탐색기](hello-android-deepdive-images/vs/02-solution-structure-sml.png)](hello-android-deepdive-images/vs/02-solution-structure.png#lightbox)

::: zone-end
::: zone pivot="macos"

다음 스크린샷에서는 솔루션의 콘텐츠를 나열합니다. Solution Pad에는 솔루션과 연결된 디렉터리 구조와 모든 파일이 포함됩니다.

[![Solution Pad](hello-android-deepdive-images/xs/02-solution-structure-sml.png)](hello-android-deepdive-images/xs/02-solution-structure.png#lightbox)

::: zone-end

**Phoneword**라는 솔루션을 만들고 Android 프로젝트 **Phoneword**를 내부에 배치했습니다.

각 폴더 및 해당 용도를 보려면 프로젝트 내부의 항목을 확인합니다.

- **속성**&ndash; 이름, 버전 번호 및 사용 권한을 포함하는 Xamarin.Android 애플리케이션에 대한 모든 요구 사항을 설명하는 [AndroidManifest.xml](~/android/platform/android-manifest.md) 파일이 포함됩니다. **속성** 폴더는 .NET 어셈블리 메타데이터 파일인 [AssemblyInfo.cs](xref:Microsoft.VisualBasic.ApplicationServices.AssemblyInfo)도 보관합니다. 이 파일을 애플리케이션에 대한 일부 기본 정보로 채우는 것이 좋습니다.

- **참조**&ndash; 애플리케이션을 빌드하고 실행하는 데 필요한 어셈블리가 포함됩니다. 참조 디렉터리를 확장하면 Xamarin의 Mono.Android 어셈블리에 대한 참조뿐만 아니라 [System](xref:System), System.Core 및 [System.Xml](xref:System.Xml)과 같은 .NET 어셈블리에 대한 참조를 확인할 수 있습니다.

- **자산**&ndash; 글꼴, 로컬 데이터 파일 및 텍스트 파일을 포함하여 애플리케이션이 실행해야 하는 파일이 포함됩니다. 여기에 포함된 파일은 생성된 `Assets` 클래스를 통해 액세스할 수 있습니다. Android 자산에 대한 자세한 내용은 Xamarin [Android 자산 사용](~/android/app-fundamentals/resources-in-android/android-assets.md) 가이드를 참조하세요.

- **리소스**&ndash; 문자열, 이미지 및 레이아웃과 같은 애플리케이션 리소스가 포함됩니다. 생성된 `Resource` 클래스를 통해 코드에 있는 이러한 리소스에 액세스할 수 있습니다. [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md) 가이드는 **리소스** 디렉터리에 대한 자세한 정보를 제공합니다. 애플리케이션 템플릿에는 **AboutResources.txt** 파일의 리소스에 대한 간단한 가이드가 포함됩니다.

### <a name="resources"></a>리소스

**리소스** 디렉터리에는 **drawable**, **layout**, **mipmap** 및 **values**라는 4개의 폴더 및 **Resource.designer.cs**라는 파일이 포함됩니다.

항목은 아래 표에 요약되어 있습니다.

- **drawable** &ndash; 드로어블 디렉터리는 이미지 및 비트맵과 같은 [드로어블 리소스](https://developer.android.com/guide/topics/resources/drawable-resource.html)를 보관합니다.

- **mipmap** &ndash; mipmap 디렉터리는 다른 시작 관리자 아이콘 밀도에 대한 드로어블 리소스를 저장합니다. 기본 템플릿에서 드로어블 디렉터리는 애플리케이션 아이콘 파일인 **Icon.png**를 보관합니다.

::: zone pivot="windows"

- **layout** &ndash; 레이아웃 디렉터리에는 각 화면이나 작업에 대한 사용자 인터페이스를 정의하는 _Android 디자이너 파일_(.axml)이 포함됩니다. 이 템플릿은 **activity_main.axml**이라는 기본 레이아웃을 만듭니다.

::: zone-end
::: zone pivot="macos"

- **layout** &ndash; 레이아웃 디렉터리에는 각 화면이나 작업에 대한 사용자 인터페이스를 정의하는 _Android 디자이너 파일_(.axml)이 포함됩니다. 이 템플릿은 **Main.axml**이라는 기본 레이아웃을 만듭니다.

::: zone-end

- **values** &ndash; 이 디렉터리는 문자열, 정수 및 색과 같은 단순 값을 저장하는 XML 파일을 보관합니다. 템플릿은 **Strings.xml**이라고 하는 문자열 값을 저장할 파일을 만듭니다.

- **Resource.designer.cs** &ndash; `Resource` 클래스라고도 하는 이 파일은 각 리소스에 할당된 고유 ID를 보유하는 partial 클래스입니다. Xamarin.Android 도구에 의해 자동으로 만들어지고 필요에 따라 다시 생성됩니다. Xamarin.Android가 수동 변경된 내용을 덮어쓰게 되면 이 파일은 수동으로 편집할 수 있습니다.

## <a name="app-fundamentals-and-architecture-basics"></a>앱 기본 항목 및 아키텍처 기본 사항

Android 애플리케이션에는 단일 진입점이 없습니다. 즉, 단일 운영 체제가 애플리케이션을 시작하기 위해 호출하는 애플리케이션에는 단일 코드 줄이 없습니다. 대신, Android 애플리케이션이 해당 클래스 중 하나를 인스턴스화할 때 애플리케이션이 시작됩니다. 이 시간 동안 Android는 전체 애플리케이션의 프로세스를 메모리로 로드합니다.

Android의 이러한 고유한 기능은 복잡한 애플리케이션을 디자인하거나 Android 운영 체제와 상호 작용할 때 매우 유용합니다. 그러나 **Phoneword** 애플리케이션과 같은 기본 시나리오를 처리할 때 이러한 옵션을 사용하면 Android가 복잡해집니다. 이러한 이유로 Android 아키텍처의 탐색이 둘로 분할됩니다. 이 가이드에서는 Android 앱: 첫 번째 화면에 가장 일반적인 진입점을 사용하는 애플리케이션을 분석합니다. [Hello, Android 멀티스크린](~/android/get-started/hello-android-multiscreen/index.md)에서 애플리케이션을 시작하는 다른 방법을 설명한 대로 Android 아키텍처의 전체 복잡성을 탐색합니다.

### <a name="phoneword-scenario---starting-with-an-activity"></a>Phoneword 시나리오 - 작업으로 시작

에뮬레이터 또는 디바이스에서 처음으로 **Phoneword** 애플리케이션을 열 때 운영 체제는 첫 번째 *작업*을 만듭니다. 작업은 단일 애플리케이션 화면에 해당하는 특별한 Android 클래스이며 사용자 인터페이스를 끌어내고 구동하는 작업을 담당합니다. Android이 애플리케이션의 첫 번째 작업을 만들 때 전체 애플리케이션을 로드합니다.

[![작업 부하](hello-android-deepdive-images/01-activity-load-sml.png)](hello-android-deepdive-images/01-activity-load.png#lightbox)

Android 애플리케이션을 통한 선형 진행이 없으므로(여러 위치에서 애플리케이션을 시작할 수 있음) Android는 고유한 방법을 클래스 및 파일이 애플리케이션을 구성하는 작업을 추적합니다. **Phoneword** 예제에서는 **Android 매니페스트**라는 특별한 XML 파일로 애플리케이션을 구성하는 모든 파트를 등록합니다. **Android 매니페스트**의 역할은 애플리케이션의 콘텐츠, 속성 및 사용 권한을 추적하고 Android 운영 체제에 공개하는 것입니다. 아래 다이어그램에 표시된 대로 단일 작업(화면)인 **Phoneword** 애플리케이션 및 Android 매니페스트 파일에 의해 서로 연결된 리소스 모음과 도우미 파일을 가정할 수 있습니다.

[![리소스 도우미](hello-android-deepdive-images/02-resources-helpers-sml.png)](hello-android-deepdive-images/02-resources-helpers.png#lightbox)

다음 섹션에서는 **Phoneword** 애플리케이션의 다양한 파트 간에 관계를 탐색합니다. 그러면 위의 다이어그램을 더 잘 이해할 수 있습니다. 이러한 탐색을 통해 Android Designer 및 레이아웃 파일에 대해 설명한 대로 사용자 인터페이스를 시작합니다.

## <a name="user-interface"></a>사용자 인터페이스

> [!TIP]
> 최신 버전의 Visual Studio에서는 Android Designer 내에서 .xml을 여는 것을 지원합니다.
>
> Android Designer에서는 .axml 파일과 .xml 파일이 모두 지원됩니다.

::: zone pivot="windows"

**activity_main.axml**은 애플리케이션의 첫 번째 화면에 대한 사용자 인터페이스 레이아웃 파일입니다. .axml은 Android Designer 파일임을 나타냅니다(AXML은 *Android XML*을 의미함). *Main*이라는 이름은 Android의 관점에서 임의입니다.&ndash; 레이아웃 파일에는 다른 이름이 지정될 수 있습니다. IDE에서 **activity_main.axml**을 열 때 *Android Designer*라는 Android 레이아웃 파일에 대한 시각적 편집기를 표시합니다.

[![Android Designer](hello-android-deepdive-images/vs/03-android-designer-sml.png "Android 디자이너")](hello-android-deepdive-images/vs/03-android-designer.png#lightbox)

**Phoneword** 앱에서 **TranslateButton**의 ID는 `@+id/TranslateButton`로 설정됩니다.

[![TranslateButton ID 설정](hello-android-deepdive-images/vs/04-translatebutton-sml.png "TranslateButton ID 설정")](hello-android-deepdive-images/vs/04-translatebutton.png#lightbox)

::: zone-end
::: zone pivot="macos"

**Main.axml**은 애플리케이션의 첫 번째 화면에 대한 사용자 인터페이스 레이아웃 파일입니다. .axml은 Android Designer 파일임을 나타냅니다(AXML은 *Android XML*을 의미함). *Main*이라는 이름은 Android의 관점에서 임의입니다.&ndash; 레이아웃 파일에는 다른 이름이 지정될 수 있습니다. IDE에서 **Main.axml**을 열 때 *Android Designer*라는 Android 레이아웃 파일에 대한 시각적 편집기를 표시합니다.

[![Android Designer](hello-android-deepdive-images/xs/03-android-designer-sml.png)](hello-android-deepdive-images/xs/03-android-designer.png#lightbox)

**Phoneword** 앱에서 **TranslateButton**의 ID는 `@+id/TranslateButton`로 설정됩니다.

[![TranslateButton ID 설정](hello-android-deepdive-images/xs/04-translatebutton-sml.png)](hello-android-deepdive-images/xs/04-translatebutton.png#lightbox)

::: zone-end

**TranslateButton**의 `id` 속성을 설정하는 경우 Android Designer는 **TranslateButton** 컨트롤을 `Resource` 클래스에 매핑하고 `TranslateButton`의 *리소스 ID*로 할당합니다. 클래스에 대한 시각적 컨트롤의 이 매핑을 사용하면 앱 코드에서 **TranslateButton** 및 다른 컨트롤을 찾아 사용할 수 있습니다. 컨트롤을 구동하는 코드에서 분리될 때 더 자세하게 다룹니다. 지금 유의해야 하는 것은 `id` 속성을 통해 컨트롤의 코드 표현이 디자이너에 있는 컨트롤의 시각적 표현에 연결되어 있다는 점입니다.

### <a name="source-view"></a>소스 뷰

디자인 화면에 정의된 모든 내용은 사용할 Xamarin.Android의 XML로 변환됩니다. Android Designer는 비주얼 디자이너에서 생성된 XML이 포함된 원본 보기를 제공합니다. 아래 스크린샷에 표시된 대로 디자이너 보기의 왼쪽 아래에 있는 **원본** 패널로 전환하여 이 XML을 볼 수 있습니다.

::: zone pivot="windows"

[![디자이너 원본 보기](hello-android-deepdive-images/vs/05-source-view-sml.png "디자이너 원본 보기")](hello-android-deepdive-images/vs/05-source-view.png#lightbox)

::: zone-end
::: zone pivot="macos"

[![디자이너 원본 보기](hello-android-deepdive-images/xs/05-source-view-sml.png)](hello-android-deepdive-images/xs/05-source-view.png#lightbox)

::: zone-end

이 XML 소스 코드에는 2개의 **TextView** 요소, 1개의 **EditText** 요소, 1개의 **Button** 요소, 총 4개의 컨트롤 요소가 포함되어야 합니다. Android Designer의 심층 안내는 Xamarin Android [Designer 개요](~/android/user-interface/android-designer/index.md) 가이드를 참조하세요.

이제 사용자 인터페이스의 시각적 부분에 대한 도구 및 개념을 설명했습니다. 다음으로 작업 및 작업 수명 주기를 탐색하여 사용자 인터페이스를 구동하는 코드로 이동하겠습니다.

## <a name="activities-and-the-activity-lifecycle"></a>작업 및 작업 수명 주기

`Activity` 클래스에는 사용자 인터페이스를 구동하는 코드가 포함됩니다.
작업은 사용자 상호 작용에 대응하고 동적 사용자 환경을 생성하는 작업을 담당합니다.
이 섹션에서는 `Activity` 클래스를 소개하고, 작업 수명 주기를 설명하고, **Phoneword** 애플리케이션에서 사용자 인터페이스를 구동하는 코드를 분석합니다.

### <a name="activity-class"></a>작업 클래스

**Phoneword** 애플리케이션에는 하나의 화면(작업)만이 있습니다. 화면을 구동하는 클래스는 `MainActivity`라고 하며 **MainActivity.cs** 파일에 배치됩니다. `MainActivity`라는 이름은 Android에서 특별한 의미가 없습니다. &ndash; 규칙이 애플리케이션에서 첫 번째 작업 이름을 지정하는 항목이지만 `MainActivity` 다른 이름을 지정하더라도 Android는 상관하지 않습니다.

**MainActivity.cs**를 열 때 `MainActivity` 클래스가 `Activity` 클래스의 *하위 클래스*이며 작업이 [작업](xref:Android.App.ActivityAttribute) 특성으로 표시됨을 확인할 수 있습니다.

```csharp
[Activity (Label = "Phone Word", MainLauncher = true)]
public class MainActivity : Activity
{
  ...
}
```

`Activity` 특성은 Android 매니페스트에서 작업을 등록합니다. 그러면 Android에서는 해당 클래스가 이 매니페스트에 의해 관리되는 **Phoneword** 애플리케이션의 일부임을 알 수 있습니다. `Label` 속성은 화면 위쪽에 표시될 텍스트를 설정합니다.

`MainLauncher` 속성에서는 Android가 애플리케이션을 시작할 때 이 작업을 표시하도록 지시합니다. 이 속성은 [Hello, Android 멀티스크린](~/android/get-started/hello-android-multiscreen/index.md) 가이드에 설명된 대로 애플리케이션에 더 많은 작업(화면)을 추가할 때 중요해집니다.

이제 `MainActivity`의 기본 사항을 다루었으므로 _작업 수명 주기_를 소개하여 작업 코드에 대해 더 자세히 알아봅니다.

### <a name="activity-lifecycle"></a>작업 수명 주기

Android에서 작업은 사용자와의 상호 작용에 따라 수명 주기의 여러 단계를 거칩니다. 작업을 만들고, 시작하고, 일시 중지하고, 다시 시작하고 소멸할 수 있습니다. `Activity` 클래스에는 시스템이 화면의 수명 주기에서 특정 시점에 호출하는 메서드가 포함되어 있습니다. 다음 다이어그램에서는 작업의 일반적인 수명뿐만 아니라 해당 수명 주기 메서드 중 일부를 보여줍니다.

[![작업 수명 주기](hello-android-deepdive-images/04-lifecycle-sml.png)](hello-android-deepdive-images/04-lifecycle.png#lightbox)

`Activity` 수명 주기 메서드를 재정의하여 작업이 로드하는 방법, 사용자에게 반응하는 방법 및 디바이스 화면에서 사라진 후에 수행되는 작업을 제어할 수 있습니다. 예를 들어 위의 다이어그램에서 수명 주기 메서드를 재정의하여 몇 가지 중요한 작업을 수행할 수 있습니다.

- **OnCreate** &ndash; 뷰를 만들고, 변수를 초기화하고, 사용자에게 작업을 표시하기 전에 수행해야 하는 기타 준비 작업을 수행합니다. 이 메서드는 작업을 메모리에 로드할 때 한 번만 호출됩니다. 

- **OnResume**&ndash; 작업이 디바이스 화면에 반환될 때마다 발생해야 하는 모든 작업을 수행합니다.

- **OnPause**&ndash; 작업이 디바이스 화면을 벗어날 때마다 발생해야 하는 모든 작업을 수행합니다.

`Activity`의 수명 주기 메서드에 사용자 지정 코드를 추가하는 경우 해당 수명 주기 메서드의 *기본 구현*을 *재정의*합니다. 기존 수명 주기 메서드(일부 코드가 이미 연결되어 있음)를 누르고 고유한 코드를 사용하여 해당 메서드를 확장합니다. 새 코드 앞에 원래 코드가 실행되도록 메서드 내에서 기본 구현을 호출합니다. 다음 섹션에서 이러한 예제를 설명합니다.

작업 수명 주기는 Android에서 중요하고 복잡한 부분입니다. _시작_ 시리즈를 완료한 후에 작업에 대한 자세한 내용을 보려는 경우 [작업 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md) 가이드를 참고하세요. 이 가이드에서는 작업 수명 주기의 첫 번째 단계인 `OnCreate`에 주목합니다.

### <a name="oncreate"></a>OnCreate

Android는 작업을 만들 때 `Activity`의 `OnCreate` 메서드를 호출합니다(화면이 사용자에게 표시되기 전에). `OnCreate` 수명 주기 메서드를 재정의하여 보기를 만들고 사용자에 맞는 작업을 준비할 수 있습니다.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);
    // Additional setup code will go here
}
```

::: zone pivot="windows"

**Phoneword** 앱의 `OnCreate`에서 가장 먼저 할 일은 Android Designer에서 만든 사용자 인터페이스를 로드하는 것입니다. UI를 로드하려면 `SetContentView`를 호출하고 레이아웃 파일(**activity_main.axml**)의 *리소스 레이아웃 이름*을 전달합니다. 레이아웃은 `Resource.Layout.activity_main`에 위치합니다.

```csharp
SetContentView (Resource.Layout.activity_main);
```

`MainActivity`가 시작되면 **activity_main.axml** 파일의 내용에 기반하는 보기를 만듭니다.

::: zone-end
::: zone pivot="macos"

**Phoneword** 앱의 `OnCreate`에서 가장 먼저 할 일은 Android Designer에서 만든 사용자 인터페이스를 로드하는 것입니다. UI를 로드하려면 `SetContentView`를 호출하고 레이아웃 파일의 *리소스 레이아웃 이름*을 전달합니다. **Main.axml**. 레이아웃은 `Resource.Layout.Main`에 위치합니다.

```csharp
SetContentView (Resource.Layout.Main);
```

`MainActivity`가 시작되면 **Main.axml** 파일의 내용에 기반하는 보기를 만듭니다. 레이아웃 파일 이름이 작업 이름과 일치합니다. &ndash; *Main*.axml은 *Main* 작업의 레이아웃입니다. 이 항목은 Android의 관점에서 필요하지 않지만 애플리케이션에 더 많은 화면을 추가할 때 이 명명 규칙을 통해 보다 쉽게 코드 파일을 레이아웃 파일과 일치시킬 수 있습니다.

::: zone-end

레이아웃 파일이 준비되면 컨트롤을 조회하기 시작할 수 있습니다.
컨트롤을 조회하려면 `FindViewById`를 호출하고 컨트롤의 리소스 ID에 전달합니다.

```csharp
EditText phoneNumberText = FindViewById<EditText>(Resource.Id.PhoneNumberText);
Button translateButton = FindViewById<Button>(Resource.Id.TranslateButton);
TextView translatedPhoneWord = FindViewById<TextView>(Resource.Id.TranslatedPhoneWord);
```

이제 레이아웃 파일에서 컨트롤에 대한 참조가 있으므로 사용자 상호 작용에 응답하도록 프로그래밍할 수 있습니다.

### <a name="responding-to-user-interaction"></a>사용자 상호 작용에 응답

Android에서 `Click` 이벤트는 사용자의 터치를 수신 대기합니다. 앱에서 람다를 사용하여 `Click` 이벤트를 처리하지만 대리자 또는 명명된 이벤트 처리기를 대신 사용할 수 있습니다. 최종 **TranslateButton** 코드는 다음과 유사합니다. 

```csharp
translateButton.Click += (sender, e) =>
{
    // Translate user's alphanumeric phone number to numeric
    translatedNumber = PhonewordTranslator.ToNumber(phoneNumberText.Text);
    if (string.IsNullOrWhiteSpace(translatedNumber))
    {
        translatedPhoneWord.Text = string.Empty;
    }
    else
    {
        translatedPhoneWord.Text = translatedNumber;
    }
};
```

## <a name="testing-deployment-and-finishing-touches"></a>터치 테스트, 배포 및 마무리

Mac용 Visual Studio와 Visual Studio 모두 애플리케이션을 테스트하고 배포하기 위한 다양한 옵션을 제공합니다. 이 섹션에서는 디버깅 옵션에 대해 다루고, 디바이스에서 애플리케이션 테스트하기에 대해 설명하며, 다양한 화면 밀도에서 사용자 지정 앱 아이콘을 만들기 위한 도구를 소개합니다.

### <a name="debugging-tools"></a>디버깅 도구

애플리케이션 코드의 문제는 진단하기가 어렵습니다. 복잡한 코드 문제를 진단하려면 [중단점을 설정](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)하거나, [코드를 단계별 실행](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)하거나, [로그 창에 정보를 출력](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)할 수 있습니다.

### <a name="deploy-to-a-device"></a>디바이스에 배포

애플리케이션을 배포하고 테스트하는 데 에뮬레이터를 사용하는 것이 좋지만 사용자는 에뮬레이터에서 최종 앱을 사용하지 않습니다. 조기에 자주 실제 디바이스에서 애플리케이션을 테스트하는 것이 좋습니다.

애플리케이션을 테스트하기 위해 Android 디바이스를 사용하기 전에 개발을 위해 구성해야 합니다. [개발용 디바이스 설정](~/android/get-started/installation/set-up-device-for-development.md) 가이드에 개발을 위한 디바이스 준비에 대한 철저한 지침을 제공합니다.

::: zone pivot="windows"

디바이스를 구성한 후에 플러그 인하고, **디바이스 선택** 대화 상자에서 선택하고, 애플리케이션을 시작하여 배포할 수 있습니다.

![디버그 디바이스 선택](hello-android-deepdive-images/vs/06-select-device.png "디버그 디바이스 선택")

::: zone-end
::: zone pivot="macos"

디바이스를 구성한 후에 플러그 인하고, **시작(플레이)** 를 누르고, **디바이스 선택** 대화 상자에서 선택하고, **확인**을 눌러서 배포할 수 있습니다.

[![디버그 디바이스 선택](hello-android-deepdive-images/xs/06-select-device-sml.png)](hello-android-deepdive-images/xs/06-select-device.png#lightbox)

::: zone-end

그러면 디바이스에서 애플리케이션을 시작합니다.

[![Phoneword 입력](hello-android-deepdive-images/05-enter-phoneword-sml.png)](hello-android-deepdive-images/05-enter-phoneword.png#lightbox)

### <a name="set-icons-for-different-screen-densities"></a>다양한 화면 밀도에 대한 아이콘 설정

Android 디바이스가 다양한 화면 크기 및 해상도에서 제공되지만 일부 이미지는 화면에서 제대로 표시되지 않습니다. 예를 들어 고밀도 Nexus 5에서 저밀도 아이콘의 스크린샷은 다음과 같습니다. 주변 아이콘에 비해 흐리게 표시되는 점에 유의하세요.

[![흐린 아이콘](hello-android-deepdive-images/06-blurry-icon-sml.png)](hello-android-deepdive-images/06-blurry-icon.png#lightbox)

이를 고려하려면 다른 해상도의 아이콘을 **리소스** 폴더에 추가하는 것이 좋습니다. Android는 다른 버전의 **mipmap** 폴더를 제공하여 밀도가 다른 시작 관리자 아이콘, 중간 밀도의 *mdpi*, 고밀도의 *hdpi* 및 고밀도 화면의 *xhdpi*, *xxhdpi*, *xxxhdpi*를 처리합니다. 다양한 크기의 아이콘을 적절한 **mipmap-** 폴더에 저장합니다.

::: zone pivot="windows"

![Mipmap 폴더](hello-android-deepdive-images/vs/07-mipmap-folders.png "Mipmap 폴더")

::: zone-end
::: zone pivot="windows"

[![Mipmap 폴더](hello-android-deepdive-images/xs/07-mipmap-folders-sml.png)](hello-android-deepdive-images/xs/07-mipmap-folders.png#lightbox)

::: zone-end

Android는 적절한 밀도의 아이콘을 선택합니다.

[![적절한 밀도의 아이콘](hello-android-deepdive-images/07-appropriate-density-sml.png)](hello-android-deepdive-images/07-appropriate-density.png#lightbox)

### <a name="generate-custom-icons"></a>사용자 지정 아이콘 생성

모든 사람이 앱을 차별화하는 데 필요한 사용자 지정 아이콘 및 시작 이미지를 만들 수 있는 디자이너를 가지고 있지는 않습니다. 다음은 사용자 앱 아트워크를 생성하는 몇 가지 대안 방법입니다.

::: zone pivot="windows"

- [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; 다른 유용한 커뮤니티 도구에 대한 링크를 포함한 모든 형식의 Android 아이콘에 대한 웹 기반 브라우저 내 생성기입니다. Google Chrome에서 가장 잘 작동합니다.

- Visual Studio &ndash; IDE에서 직접 앱에 대한 간단한 아이콘 집합을 만드는 데 사용할 수 있습니다.

- [Glyphish](https://www.glyphish.com/) &ndash; 무료 다운로드 및 구매가 가능한 고품질의 미리 빌드된 아이콘 집합입니다.

- [Fiverr](https://www.fiverr.com/) &ndash; 다양한 디자이너를 선택하여 자신에게 맞는 아이콘 집합을 만들 수 있습니다($5부터). 성공할 수도 있고 아니면 실패할 수도 있지만, 즉시 디자인된 아이콘이 필요한 경우에 좋은 리소스입니다.

::: zone-end
::: zone pivot="macos"

- [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/index.html) &ndash; 다른 유용한 커뮤니티 도구에 대한 링크를 포함한 모든 형식의 Android 아이콘에 대한 웹 기반 브라우저 내 생성기입니다. Google Chrome에서 가장 잘 작동합니다.

- [Sketch 3](https://itunes.apple.com/us/app/sketch/id852320343?mt=12) &ndash; Sketch는 사용자 인터페이스, 아이콘 등을 디자인하기 위한 Mac 앱입니다. Xamarin 앱 아이콘 및 시작 이미지 집합을 디자인하는 데 사용되었던 앱입니다. Sketch 3은 App Store에서 제공됩니다(약 $80). 체험용 [Sketch Tool](https://bohemiancoding.com/sketch/tool/)도 사용해 볼 수 있습니다.

- [Pixelmator](https://www.pixelmator.com/) &ndash; Mac용 다양한 이미지 편집 앱입니다(약 $30).

- [Glyphish](https://www.glyphish.com/) &ndash; 무료 다운로드 및 구매가 가능한 고품질의 미리 빌드된 아이콘 집합입니다.

- [Fiverr](https://www.fiverr.com/) &ndash; 다양한 디자이너를 선택하여 자신에게 맞는 아이콘 집합을 만들 수 있습니다($5부터). 성공할 수도 있고 아니면 실패할 수도 있지만, 즉시 디자인된 아이콘이 필요한 경우에 좋은 리소스입니다.

::: zone-end

아이콘 크기 및 요구 사항에 대한 자세한 내용은 [Android 리소스](~/android/app-fundamentals/resources-in-android/index.md) 가이드를 참조하세요.

::: zone pivot="macos"

### <a name="adding-google-play-services-packages"></a>Google Play 서비스 패키지 추가

_Google Play 서비스_는 Android 개발자가 Google 맵, Google Cloud Messaging 및 앱 내 청구 등 Google에서 최신 기능을 활용할 수 있도록 하는 추가 기능 라이브러리 집합입니다.
이전에는 모든 Google Play 서비스 라이브러리에 대한 바인딩을 Xamarin에서 단일 패키지의 형태로 제공했습니다. &ndash; Mac용 Visual Studio부터 앱에 포함할 Google Play 서비스를 패키지하도록 선택하는 새 프로젝트 대화 상자가 제공됩니다.

하나 이상의 Google Play 서비스 라이브러리를 추가하려면 프로젝트 트리에서 **패키지** 노드를 마우스 오른쪽 단추로 클릭하고, **Google Play 서비스 추가...** 를 클릭합니다.

[![Google Play 서비스 추가](hello-android-deepdive-images/xs/08-add-google-play-services-sml.png)](hello-android-deepdive-images/xs/08-add-google-play-services.png#lightbox)

**Google Play 서비스 추가** 대화 상자가 표시되면 프로젝트에 추가하려는 패키지(NuGet)를 선택합니다.

[![패키지 선택](hello-android-deepdive-images/xs/09-add-dialog-sml.png)](hello-android-deepdive-images/xs/09-add-dialog.png#lightbox)

서비스를 선택하고 **패키지 추가**를 클릭하면 Mac용 Visual Studio가 필요한 모든 종속 Google Play 서비스뿐만 아니라 선택한 패키지를 다운로드하고 설치합니다. 일부 경우에는 패키지를 설치하기 전에 **동의**를 클릭해야 하는 **라이선스 승인** 대화 상자가 표시될 수 있습니다.

[![라이선스 승인](hello-android-deepdive-images/xs/10-license-acceptance-sml.png)](hello-android-deepdive-images/xs/10-license-acceptance.png#lightbox)

::: zone-end

## <a name="summary"></a>요약

지금까지 이제 Xamarin.Android 애플리케이션의 구성 요소와 이를 만드는 데 필요한 도구에 대해 확실하게 이해했습니다.

_시작_ 시리즈의 다음 자습서에서는 더 많은 고급 Android 아키텍처 및 개념에 대해 알아보기 위해 여러 화면을 처리하도록 애플리케이션을 확장합니다.
