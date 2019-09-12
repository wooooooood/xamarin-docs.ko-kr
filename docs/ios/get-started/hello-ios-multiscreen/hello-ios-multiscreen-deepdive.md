---
title: Hello, iOS 멀티스크린 - 심층 분석
description: 이 문서에서는 확장된 Phoneword 애플리케이션, 더 고려할 모델-뷰-컨트롤러, iOS 탐색 및 기타 iOS 개발 개념을 심층적으로 살펴봅니다.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: c866e5f4-8154-4342-876e-efa0693d66f5
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/05/2018
ms.openlocfilehash: 72e421e088a582e4d2de1cf830a0978cca9f45c8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762650"
---
# <a name="hello-ios-multiscreen--deep-dive"></a>Hello, iOS 멀티스크린 - 심층 분석

빠른 시작 연습에서는 첫 번째 멀티스크린 Xamarin.iOS 애플리케이션을 빌드하고 실행했습니다. 이제 iOS 탐색 및 아키텍처에 대한 깊은 이해를 개발할 시간입니다.

이 가이드에서는 *MVC(모델, 뷰, 컨트롤러)* 패턴 및 iOS 아키텍처 및 탐색에서의 해당 역할을 소개합니다.
그런 다음, 탐색 컨트롤러에 대해 자세히 알아보고 iOS에서 친숙한 탐색 환경을 제공하기 위해 사용하는 방법을 알아봅니다.

## <a name="model-view-controller-mvc"></a>MVC(모델-뷰-컨트롤러)

[Hello, iOS](~/ios/get-started/hello-ios/index.md) 자습서에서는 iOS 애플리케이션에 뷰 컨트롤러가 해당 *콘텐츠 뷰 계층 구조*를 창으로 로딩할 책임이 있는 하나의 *창*만 있다는 것을 배웠습니다. 두 번째 Phoneword 연습에서는 애플리케이션에 두 번째 화면을 추가하고 아래 다이어그램에 표시된 것처럼 두 개의 화면 간에 일부 데이터(전화번호 목록)를 전달했습니다.

 [![](hello-ios-multiscreen-deepdive-images/08.png "이 다이어그램은 두 개의 화면 간 데이터 전달을 보여줍니다.")](hello-ios-multiscreen-deepdive-images/08.png#lightbox)

예제에서 데이터는 첫 번째 화면에서 수집되었고, 첫 번째 뷰 컨트롤러에서 두 번째로 전달되었으며 두 번째 화면에서 표시되었습니다. 이 화면, 뷰 컨트롤러 및 데이터의 분리는 *MVC(모델, 뷰, 컨트롤러)* 패턴을 따릅니다. 다음 단원에서는 패턴의 이점, 해당 구성 요소 및 Phoneword 애플리케이션에서 사용하는 방법을 설명합니다.

### <a name="benefits-of-the-mvc-pattern"></a>MVC 패턴의 이점

모델-뷰-컨트롤러는 *디자인 패턴*입니다. 코드의 일반적인 문제 또는 사용 사례에 대한 재사용 가능한 아키텍처 솔루션입니다. MVC는 *GUI(그래픽 사용자 인터페이스)* 를 포함하는 애플리케이션에 대한 아키텍처입니다. 세 가지 역할 중 하나에 애플리케이션의 개체를 할당합니다(*모델*(데이터 또는 애플리케이션 논리), *뷰*(사용자 인터페이스) 및 *컨트롤러*(코드 숨김)). 아래 다이어그램에서는 세 가지의 MVC 패턴과 사용자 간의 관계를 보여 줍니다.

 [![](hello-ios-multiscreen-deepdive-images/00.png "이 다이어그램에서는 세 가지의 MVC 패턴과 사용자 간의 관계를 보여줍니다.")](hello-ios-multiscreen-deepdive-images/00.png#lightbox)

MVC 패턴은 GUI 애플리케이션의 다른 부분 간의 논리 분리를 제공하고 코드 및 뷰를 쉽게 다시 사용할 수 있도록 하기 때문에 유용합니다. 시작하여 세 가지 역할을 각각 자세히 살펴보겠습니다.

> [!NOTE]
> MVC 패턴은 ASP.NET 페이지 또는 WPF 애플리케이션의 구조와 대략 비슷합니다. 이러한 예제에서 뷰는 실제로 UI를 설명할 책임이 있는 구성 요소이며, ASP.NET에서 ASPX(HTML) 페이지 또는 WPF 애플리케이션에서 XAML에 해당합니다. 컨트롤러는 뷰를 관리할 책임이 있는 구성 요소입니다. 이는 ASP.NET 또는 WPF에서 코드 숨김에 해당합니다.

### <a name="model"></a>모델

모델 개체는 일반적으로 뷰로 표시되거나 입력되는 데이터의 애플리케이션별 표현입니다. 모델은 종종 느슨하게 정의됩니다. 예를 들어 **Phoneword_iOS** 앱에서 전화번호 목록(문자열 목록으로 표시됨)이 모델입니다. 플랫폼 간 애플리케이션을 빌드한 경우 iOS와 Android 애플리케이션 사이에 **PhonewordTranslator** 코드를 공유하도록 선택할 수 있습니다. 해당 공유 코드를 모델로도 고려해 볼 수 있습니다.

MVC는 *데이터 지속성* 및 모델의 *액세스*와 완전히 독립적입니다. 즉, MVC는 데이터의 모양 또는 저장되는 방법을 신경 쓰지 않으며 데이터가 *표현*되는 방법만 신경 씁니다. 예를 들어 SQL 데이터베이스에 데이터를 저장하거나, 일부 클라우드 스토리지 메커니즘에 보관하거나, 단순히 `List<string>`을 사용하도록 선택할 수 있습니다. MVC 목적을 위해 자체 데이터 표현만 패턴에 포함되어 있습니다.

일부 경우에서 MVC의 모델 부분은 비어 있을 수 있습니다. 예를 들어 전화 변환기의 작동 방식, 빌드한 이유 및 버그를 보고하기 위해 연락하는 방법을 설명하는 몇 가지 정적 페이지를 앱에 추가하도록 선택할 수도 있습니다. 이러한 앱 화면은 여전히 뷰 및 컨트롤러를 사용하여 만들어지지만 실제 모델 데이터가 없을 것입니다.

> [!NOTE]
> 일부 문서에서 MVC 패턴의 모델 부분은 UI에 표시되는 데이터뿐 아니라 전체 애플리케이션 백 엔드를 참조할 수 있습니다. 이 가이드에서는 모델의 최신 해석을 사용하지만 차이는 특별히 중요하지 않습니다.

### <a name="view"></a>보기

뷰는 사용자 인터페이스를 렌더링할 책임이 있는 구성 요소입니다. MVC 패턴을 사용하는 거의 모든 플랫폼에서 사용자 인터페이스는 뷰의 계층 구조로 구성됩니다. MVC의 뷰를 계층 구조의 상단에서 단일 뷰(루트 뷰로 알려짐)가 있는 뷰 계층 구조 및 그 아래의 개수에 관계 없는 자식 뷰(하위 뷰로 알려짐)로 생각할 수 있습니다. iOS에서 화면의 콘텐츠 뷰 계층 구조는 MVC에서 뷰 구성 요소에 해당합니다.

### <a name="controller"></a>컨트롤러

컨트롤러 개체는 모든 항목을 함께 연결하는 구성 요소이며 `UIViewController`에 의해 iOS에서 표시됩니다. 컨트롤러를 화면 또는 뷰 집합에 대한 백업 코드로 생각할 수 있습니다. 컨트롤러는 사용자로부터 요청을 수신하고 적절한 뷰 계층 구조를 반환하는 일을 담당합니다. 뷰에서 요청을 수신하고(단추 클릭, 텍스트 입력 등) 적절한 처리, 뷰 수정 및 뷰 다시 로드를 수행합니다. 또한 컨트롤러는 애플리케이션에 있는 모든 백업 데이터 저장소에서 모델을 만들거나 검색하고 해당 데이터로 뷰를 채우는 일을 담당합니다.

컨트롤러는 다른 컨트롤러를 관리할 수도 있습니다. 예를 들어 하나의 컨트롤러는 다른 화면을 표시하거나 해당 순서 및 화면 간 전환을 모니터링하도록 컨트롤러의 스택을 관리해야 하는 경우 다른 컨트롤러를 로드할 수도 있습니다. 다음 섹션에서는 *탐색 컨트롤러*라는 특수한 유형의 iOS 뷰 컨트롤러를 도입할 때 다른 컨트롤러를 관리하는 컨트롤러의 예를 살펴봅니다.

## <a name="navigation-controller"></a>탐색 컨트롤러

Phoneword 애플리케이션에서는 여러 화면 간 탐색을 관리하는 데 도움을 주도록 탐색 컨트롤러를 사용했습니다. 탐색 컨트롤러는 `UINavigationController` 클래스로 표시되는 특수화된 `UIViewController`입니다. 단일 콘텐츠 뷰 계층 구조를 관리하는 대신 탐색 컨트롤러는 다른 뷰 컨트롤러 및 제목, 뒤로 단추 및 다른 선택적 기능을 포함하는 탐색 도구 모음의 형식으로 자체의 특수한 콘텐츠 뷰 계층 구조를 관리합니다.

탐색 컨트롤러는 iOS 애플리케이션에서 일반적이며 아래 스크린샷에서 표시된 것처럼 **Settings** 앱과 같은 주요한 iOS 애플리케이션에 대한 탐색을 제공합니다.

 [![](hello-ios-multiscreen-deepdive-images/01.png "탐색 컨트롤러는 여기에 표시된 Settings 앱과 같은 iOS 애플리케이션에 대한 탐색을 제공합니다.")](hello-ios-multiscreen-deepdive-images/01.png#lightbox)

탐색 컨트롤러는 세 가지 기본 기능을 제공합니다.

- **앞으로 후크 탐색을 제공합니다.** – 탐색 컨트롤러는 콘텐츠 뷰 계층 구조가 *탐색 스택*으로 *푸시*되는 계층적 탐색 메타포를 사용합니다. 탐색 스택을 아래 다이어그램에 표시된 것처럼 최상위 카드만 표시되는 플레잉 카드의 스택으로 생각할 수 있습니다.  

    [![](hello-ios-multiscreen-deepdive-images/02.png "이 다이어그램은 카드의 스택으로 탐색을 보여줍니다.")](hello-ios-multiscreen-deepdive-images/02.png#lightbox)

- **필요한 경우 뒤로 단추를 제공합니다.** - 새 항목을 탐색 스택으로 푸시할 때 제목 표시줄은 사용자를 뒤로 탐색할 수 있도록 하는 *뒤로 단추*를 자동으로 표시할 수 있습니다. 뒤로 단추를 누르면 탐색 스택에서 현재 뷰 컨트롤러를 *꺼내고* 이전 콘텐츠 뷰 계층 구조를 창으로 로드합니다.  

    [![](hello-ios-multiscreen-deepdive-images/03.png "이 다이어그램은 스택에서 카드 '꺼내기'를 보여줍니다.")](hello-ios-multiscreen-deepdive-images/03.png#lightbox)

- **제목 표시줄을 제공합니다.** – 탐색 컨트롤러의 윗 부분은 *제목 표시줄*이라고 합니다. 아래 다이어그램에 표시된 것처럼 뷰 컨트롤러 제목을 표시합니다.  

    [![](hello-ios-multiscreen-deepdive-images/04.png "제목 표시줄은 뷰 컨트롤러 제목을 표시합니다.")](hello-ios-multiscreen-deepdive-images/04.png#lightbox)

### <a name="root-view-controller"></a>루트 뷰 컨트롤러

탐색 컨트롤러는 콘텐츠 뷰 계층 구조를 관리하지 않으므로 표시할 항목이 없습니다.
대신 탐색 컨트롤러는 *루트 뷰 컨트롤러*와 쌍을 이룹니다.

 [![](hello-ios-multiscreen-deepdive-images/05.png "탐색 컨트롤러는 루트 뷰 컨트롤러와 쌍을 이룹니다.")](hello-ios-multiscreen-deepdive-images/05.png#lightbox)

루트 뷰 컨트롤러는 탐색 컨트롤러의 스택에서 첫 번째 뷰 컨트롤러를 나타내고, 루트 뷰 컨트롤러의 콘텐츠 뷰 계층 구조는 창에 로드되는 첫 번째 콘텐츠 뷰 계층 구조입니다. 전체 애플리케이션을 탐색 컨트롤러의 스택에 배치하고자 하는 경우 원본 없는 Segue를 탐색 컨트롤러로 이동시키고 Phoneword 앱에서 수행한 것처럼 첫 번째 화면의 뷰 컨트롤러를 루트 뷰 컨트롤러로 설정할 수 있습니다.

 [![](hello-ios-multiscreen-deepdive-images/06.png "원본 없는 Segue는 첫 번째 화면 뷰 컨트롤러를 루트 뷰 컨트롤러로 설정합니다.")](hello-ios-multiscreen-deepdive-images/06.png#lightbox)

### <a name="additional-navigation-options"></a>추가 탐색 옵션

탐색 컨트롤러는 iOS에서 탐색을 처리하는 일반적인 방법이지만 유일한 옵션은 아닙니다. 예를 들어, [탭 표시줄 컨트롤러](~/ios/user-interface/controls/creating-tabbed-applications.md)는 서로 다른 기능 영역으로 애플리케이션을 분할할 수 있고, [분할 뷰 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/split_view/use_split_view_to_show_two_controllers)는 마스터/세부 정보 보기를 만드는 데 사용될 수 있습니다. 이러한 다른 탐색 패러다임을 사용하여 탐색 컨트롤러를 결합하면 iOS에서 콘텐츠를 나타내고 탐색하는 여러 유연한 방법을 제공할 수 있습니다.

## <a name="handling-transitions"></a>전환 처리

Phoneword 연습에서는 두 가지 방법으로(Storyboard Segue로 먼저 그 다음에 프로그래밍 방식으로) 두 뷰 컨트롤러 간 전환을 처리했습니다. 이러한 두 옵션을 자세히 살펴보겠습니다.

### <a name="prepareforsegue"></a>PrepareForSegue

**표시** 작업으로 Segue를 Storyboard에 추가할 때 두 번째 뷰 컨트롤러를 탐색 컨트롤러의 스택으로 푸시하도록 iOS에 지시합니다.

 [![](hello-ios-multiscreen-deepdive-images/09.png "드롭다운 목록에서 Segue 형식 설정")](hello-ios-multiscreen-deepdive-images/09.png#lightbox)

Segue를 Storyboard에 추가하는 것은 화면 간의 간단한 전환을 만들기에 충분합니다. 뷰 컨트롤러 간에 데이터를 전달하려는 경우 `PrepareForSegue` 메서드를 재정의하고 데이터를 직접 처리해야 합니다.

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);
    ...
}
```

iOS는 전환이 발생하기 직전에 `PrepareForSegue`를 호출하고 Storyboard에서 만든 Segue를 메서드에 전달합니다.
이 시점에서 Segue의 대상 뷰 컨트롤러를 수동으로 설정해야 합니다. 다음 코드는 대상 뷰 컨트롤러로 핸들을 가져오고 적절한 클래스(이 경우 CallHistoryController)로 캐스팅합니다.

```csharp
CallHistoryController callHistoryController = segue.DestinationViewController as CallHistoryController;
```

마지막으로 `CallHistoryController`의 `PhoneHistory` 속성을 전화를 건 전화번호 목록으로 설정하여 `ViewController`에서 `CallHistoryController`로 전화번호 목록(모델)을 전달합니다.

```csharp
callHistoryController.PhoneNumbers = PhoneNumbers;
```

Segue를 사용하여 데이터를 전달하기 위한 전체 코드는 다음과 같습니다.

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController as CallHistoryController;

    if (callHistoryController != null) {
         callHistoryController.PhoneNumbers = PhoneNumbers;
    }
 }
```

### <a name="navigation-without-segues"></a>Segue 없이 탐색

첫 번째 뷰 컨트롤러에서 코드의 두 번째로 전환하는 것은 Segue와 동일한 프로세스이지만 몇 단계는 수동으로 완료해야 합니다.
먼저 `this.NavigationController`를 사용하여 현재 해당하는 스택의 탐색 컨트롤러에 대한 참조를 가져옵니다. 그런 다음, 탐색 컨트롤러의 `PushViewController` 메서드를 사용하여 뷰 컨트롤러 및 전환에 애니메이션을 적용할 옵션을 전달하여 다음 뷰 컨트롤러를 스택으로 수동으로 푸시합니다(이를 `true`로 설정).

다음 코드는 Phoneword 화면에서 통화 기록 화면으로 전환을 처리합니다.

```csharp
this.NavigationController.PushViewController (callHistory, true);
```

다음 뷰 컨트롤러로 전환하려면 `this.Storyboard.InstantiateViewController`를 호출하고 `CallHistoryController`의 Storyboard ID를 전달하여 Storyboard에서 수동으로 인스턴스화해야 합니다.

```csharp
CallHistoryController callHistory =
this.Storyboard.InstantiateViewController
("CallHistoryController") as CallHistoryController;
```

마지막으로 Segue로 전환을 처리할 때 수행한 것처럼 `CallHistoryController`의 `PhoneHistory` 속성을 전화를 건 전화번호 목록으로 설정하여 `ViewController`에서 `CallHistoryController`로 전화번호 목록(모델)을 전달합니다.

```csharp
callHistory.PhoneNumbers = PhoneNumbers;
```

프로그래밍 방식 전환에 대한 전체 코드는 다음과 같습니다.

```csharp
CallHistoryButton.TouchUpInside += (object sender, EventArgs e) => {
    // Launches a new instance of CallHistoryController
    CallHistoryController callHistory = this.Storyboard.InstantiateViewController ("CallHistoryController") as CallHistoryController;
    if (callHistory != null) {
     callHistory.PhoneNumbers = PhoneNumbers;
     this.NavigationController.PushViewController (callHistory, true);
    }
};
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword에 도입된 추가 개념

Phoneword 애플리케이션에는 이 가이드에서 다루지 않은 몇 가지 개념이 도입되었습니다. 이러한 개념은 다음과 같습니다.

- **뷰 컨트롤러의 자동 생성** – **Properties Pad**에 뷰 컨트롤러에 대한 클래스 이름을 입력할 때 iOS 디자이너는 해당 클래스가 있는지 확인한 다음, 클래스를 백업하는 뷰 컨트롤러를 생성합니다. 해당 항목 및 다른 iOS 디자이너 기능에 대한 자세한 내용은 [iOS 디자이너 소개](~/ios/user-interface/designer/introduction.md) 가이드를 참조하세요.
- **테이블 뷰 컨트롤러** – `CallHistoryController`는 테이블 뷰 컨트롤러입니다. 테이블 뷰 컨트롤러는 iOS에서 가장 일반적인 레이아웃 및 데이터 표시 도구인 테이블 뷰를 포함합니다. 테이블은 이 가이드의 범위를 벗어납니다. 테이블 뷰 컨트롤러에 대한 자세한 내용은 [테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md) 가이드를 참조하세요.
- **Storyboard ID** – Storyboard ID를 설정하면 Storyboard에서 뷰 컨트롤러에 대한 코드 숨김을 포함하는 Objective-C에서 뷰 컨트롤러 클래스를 만듭니다. Storyboard ID를 사용하여 Objective-C 클래스를 찾고 Storyboard에서 뷰 컨트롤러를 인스턴스화합니다. Storyboard ID에 대한 자세한 내용은 [스토리보드 소개](~/ios/user-interface/storyboards/index.md) 가이드를 참조하세요.

## <a name="summary"></a>요약

첫 번째 멀티스크린 iOS 애플리케이션을 완성한 것을 축하합니다!

이 가이드에서는 MVC 패턴을 소개했고 이를 사용하여 멀티스크린 애플리케이션을 만들었습니다. 또한 탐색 컨트롤러 및 iOS 탐색을 구동하는 해당 역할을 살펴봤습니다. 이제 고유한 Xamarin.iOS 애플리케이션 개발을 시작할 견고한 기반이 마련되었습니다.

다음으로 [모바일 개발 소개](~/cross-platform/get-started/introduction-to-mobile-development.md) 및 [플랫폼 간 애플리케이션 빌드](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) 가이드를 사용하여 Xamarin으로 플랫폼 간 애플리케이션 빌드를 알아보겠습니다.

## <a name="related-links"></a>관련 링크

- [Hello, iOS(샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)
- [iOS 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS 프로비전 포털](https://developer.apple.com/ios/manage/overview/index.action)
