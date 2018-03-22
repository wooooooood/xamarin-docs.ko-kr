---
title: "통합 된 스토리 보드"
description: "통합 된 스토리 보드를 사용 하는 iOS 장치 화면 크기의 확장 범위를 포함 하려면 여러 스토리 보드를 사용 하지 않고 단일 스토리 보드를 사용 하 여 사용자 인터페이스를 만들려면 개발자 있습니다. 이 문서는 Xamarin.iOS 내에서 통합 된 스토리 보드의 작업으로 깊은 개요를 제공 하도록 설계 되었습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 60b2e6fa65226631fe2d2c847a56852ac9ae63d2
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="unified-storyboards"></a>통합 된 스토리 보드

_통합 된 스토리 보드를 사용 하는 iOS 장치 화면 크기의 확장 범위를 포함 하려면 여러 스토리 보드를 사용 하지 않고 단일 스토리 보드를 사용 하 여 사용자 인터페이스를 만들려면 개발자 있습니다. 이 문서는 Xamarin.iOS 내에서 통합 된 스토리 보드의 작업으로 깊은 개요를 제공 하도록 설계 되었습니다._

iOS 8 사용자 인터페이스를 만들기 위한 새, 사용 가능한 더 간단한 메커니즘을 포함-통합 된 스토리 보드 합니다. 모든 다른 하드웨어 화면 크기 처리 하려면 단일 스토리 보드와 뷰를 신속 하 고 응답에서 만들 수는 "디자인-한 번, 사용 하 여 다" 스타일입니다.

개발자는 더 이상 별도 및 특정 스토리 보드를 만드는 iPhone 및 iPad 장치에 대 한 요구 사항이, 공용 인터페이스와 함께 응용 프로그램을 디자인 하 고 다음 서로 다른 크기 클래스에 대 한 해당 인터페이스를 사용자 지정할 수 있는 유연성을 갖게 됩니다. 이러한 방식으로 응용 프로그램 폼 팩터 각 수준에 적용할 수 있습니다 및 최상의 환경을 제공 하기 위해 각 사용자 인터페이스를 튜닝할 수 있습니다.

<a name="size-classes" />

## <a name="size-classes"></a>크기 클래스

IOS 8 이전 버전에서 개발자는 다음과 같이 사용 됩니다. `UIInterfaceOrientation` 및 `UIInterfaceIdiom` iPhone 및 iPad 장치 간에 가로 세로 모드 간을 구분 하 합니다. IOS8, 방향 및 장치가 결정 됩니다 사용 하 여 *크기 클래스*합니다.

장치 세로 및 가로 축에서 크기 클래스에 의해 정의 된 및 iOS 8의에서 크기 클래스의 두 종류가 있습니다.

-  **일반** – 큰 화면 크기 (예: iPad) 또는 크기를 크게을 느낄 수 가젯을 중 하나입니다 (예:는 `UIScrollView`
-  **Compact** -작은 장치 (iPhone) 등입니다. 이 크기는 장치의 방향을 고려합니다.


두 가지 개념을 함께 사용할 경우 결과 다음 다이어그램에 표시 되는 모두 서로 다른 방향에 사용할 수 있는 다양 한 가능한 크기를 정의 하는 2 x 2 표:

 [![](unified-storyboards-images/sizeclassgrid.png "일반 연결 및 압축 방향에서 사용할 수 있는 다양 한 가능한 크기를 정의 하는 2 x 2 표")](unified-storyboards-images/sizeclassgrid.png#lightbox)

개발자 (처럼 위의 그래픽) 서로 다른 레이아웃으로 발생 하는 네 가지 가능성 중 하나를 사용 하는 보기 컨트롤러를 만들 수 있습니다.

### <a name="ipad-size-classes"></a>iPad 크기 클래스

IPad의 경우 크기 때문에 **일반** 양쪽 방향에 대 한 크기 클래스입니다.

 [![](unified-storyboards-images/image1.png "iPad 크기 클래스")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone 크기 클래스

IPhone에 장치의 방향에 따라 서로 다른 크기 클래스가 있습니다.

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone 크기 클래스")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  화면에 장치가 세로 모드일 때는 **압축** 가로로 클래스 및 **일반** 세로로
-  가로 모드로 장치가 때 화면 클래스 세로 모드에서 취소 됩니다.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus 크기 클래스

크기는 가로 방향의 다르지만 세로 방향으로 작업 하는 경우 이전 Iphone과 동일 합니다.

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus 크기 클래스")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

때문에 iPhone 6 더하기 화면이 작아 화면이 이므로 가로 모드에서 일반 너비 크기 클래스를 사용할 수 있습니다.

### <a name="support-for-a-new-screen-scale"></a>새 화면 배율에 대 한 지원

IPhone 6 더하기 (세 번 원래 iPhone 화면 해상도) 3.0의 화면 배율 인수 인 새 HD 레 티 나 디스플레이 사용 합니다. 이러한 장치에서 가능한 최상의 환경을 제공 하려면이 화면 눈금에 대 한 설계 된 새 아트 워크를 포함 합니다. 카탈로그 1 x, 2 배 및 3 가지 x 크기;에서 이미지를 포함할 수 이상 버전에서 Xcode 6, 자산 새 이미지 자산 및 iOS iPhone 6에서 실행 하는 경우 올바른 자산을 선택 합니다를 추가 하기만 하면 플러스 합니다.

또한 iOS에서 동작을 로드 하는 이미지 인식는 `@3x` 이미지 파일에는 접미사입니다. 예를 들어, 개발자는 다음 파일 이름 가진 응용 프로그램의 번들의 이미지 자산 (해상도 서로 다른)를 포함 하는 경우: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, 및 `MonkeyIcon@3x.png`합니다. Iphone 6 Plus의 `MonkeyIcon@3x.png` 이미지는 자동으로 개발자는 다음 코드를 사용 하 여 이미지를 로드 하는 경우:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
IOS를 사용 하는 UI 요소에 이미지를 할당 하는 경우 또는으로 디자이너 `MonkeyIcon.png`, `MonkeyIcon@3x.png` 에 사용 될, 자동으로 다시 iPhone 6 Plus입니다.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>동적 시작 화면

시작 화면 파일은 응용 프로그램은 실제로 시작 접속 하는 사용자에 게 피드백을 제공 하는 iOS 응용 프로그램을 시작할 동안 시작 화면으로 표시 됩니다. IOS 8 하기 전에 개발자에 여러 포함 해야 `Default.png` 자산에서 실행 되 고 응용 프로그램이 각 장치 유형, 방향 및 화면 해상도 대 한 이미지입니다.

새로운 개발자 iOS 8에는 단일 원자 단위를 만들 수 `.xib` 자동 레이아웃 및 크기 클래스를 사용 하는 Xcode에서 파일을 *동적 시작 화면* 모든 장치, 해상도 및 방향에 대 한 작동 합니다. 이 뿐만 아니라 만들고 모든 필요한 이미지 자산, 유지 관리 하는 개발자는 데 필요한 작업 양을 줄어들지만 응용 프로그램의 설치 된 번들의 크기를 줄입니다.

## <a name="traits"></a>특성

특성은 해당 환경이 변경 되 면 레이아웃을 어떻게 변경 되는지 확인 하려면 사용할 수 있는 속성입니다. 속성 집합으로 구성 됩니다 (의 `HorizontalSizeClass` 및 `VerticalSizeClass` 기반 `UIUserInterfaceSizeClass`), 인터페이스 관용구 뿐만 아니라 ( `UIUserInterfaceIdiom`) 및 표시 배율을 합니다.

모든 위의 상태에에서 래핑됩니다.이 Apple 특성 컬렉션으로 참조 하는 컨테이너 ( `UITraitCollection`), 속성 뿐만 아니라 해당 값도 포함 된 합니다.

## <a name="trait-environment"></a>환경 특성

특성 환경 iOS 8의에서 새로운 인터페이스 이며 다음과 같은 개체에 대 한 특성 컬렉션을 반환할 수 있습니다.

-  화면 ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  컨트롤러 보기 ( `UIViewController` ).
-  보기 ( `UIView` ).
-  프레젠테이션 컨트롤러 ( `UIPresentationController` ).


개발자 성분 환경에 의해 반환 되는 특성 컬렉션을 사용 하 여 사용자 인터페이스를 배치 하는 방법을 결정 합니다.

모든 특성 환경 다음 다이어그램에 표시 된 대로 계층 구조를 확인 합니다.

 [![](unified-storyboards-images/viewhierarchy.png "환경 특성 계층 구조 다이어그램")](unified-storyboards-images/viewhierarchy.png#lightbox)

특성 컬렉션의 부모를 자식 환경에서 기본적으로 흐름는 위의 특성 환경의 포함 하는 합니다.

현재 특성 컬렉션을 가져와야 할 뿐만 아니라 성분 환경에는 `TraitCollectionDidChange` 메서드 보기 또는 뷰-컨트롤러 서브 클래스에서 재정의 될 수 있는 합니다. 개발자는 해당 특성이 변경 된 경우 특성에 의존 하는 UI 요소 중 하나를 수정 하려면이 메서드를 사용할 수 있습니다.

## <a name="typical-trait-collections"></a>일반적인 특성 컬렉션입니다.

이 섹션에서는 일반적인 유형의 사용자는 iOS 8 사용 하면 발생할 수 있는 특성 컬렉션을 설명 합니다.

다음은 개발자 iPhone에서 볼 수 있는 일반적인 특성 컬렉션입니다.

|속성|값|
|--- |--- |
|`HorizontalSizeClass`|압축|
|`VerticalSizeClass`|기본|
|`UserInterfaceIdom`|전화 번호|
|`DisplayScale`|2.0|

위의 집합은 모든 특성 속성에 대 한 값이 완전히 정규화 된 특성 컬렉션을 나타냅니다.

것도 가능 특성 컬렉션을 일부 값이 누락 되었습니다 (Apple로 참조 하는 *Unspecified*):

|속성|값|
|--- |--- |
|`HorizontalSizeClass`|압축|
|`VerticalSizeClass`|지정되지 않음|
|`UserInterfaceIdom`|지정되지 않음|
|`DisplayScale`|지정되지 않음|

그러나 일반적으로 개발자는 특성 컬렉션을 성분 환경 요청 반환 됩니다 정규화 컬렉션 위의 예제에서와 같이 합니다.

현재 뷰 계층 구조 내에서 (예: 보기 또는 뷰-컨트롤러) 성분 환경 없으면 개발자 발생할 수 다시 지정 되지 않은 값 하나 이상의 특성 속성에 대 한 합니다.

개발자와 같은, Apple에서 제공한 만들기 방법 중 하나를 사용 하는 경우 부분적으로 정규화 된 특성 컬렉션을 발생 하기도 합니다 `UITraitCollection.FromHorizontalSizeClass`, 새 컬렉션을 만들 수 있습니다.

여러 특성 컬렉션에 대해 수행할 수 있는 한 번의 작업은 비교 하기를 서로 다른 경우 포함 된 특성 컬렉션을 하나 요청 포함 됩니다는 합니다. 의미 *제약* 즉, 두 번째 컬렉션에 지정 된 모든 특성에 대 한 값이 일치 해야 첫 번째 컬렉션의 값과 정확 하 게 됩니다.

테스트 하려면 두 개의 특성 사용에서 `Contains` 의 메서드는 `UITraitCollection` 테스트할 특성의 값을 전달 합니다.

개발자 수동으로 수행할 수 비교를 확인 하기 위해 코드에서 레이아웃 보기 또는 뷰 컨트롤러 하는 방법입니다. 그러나 `UIKit` 예를 들어 일부 모양 프록시에서와 같이 기능을 제공 하기 내부적으로이 메서드를 사용 합니다.

## <a name="appearance-proxy"></a>모양 프록시

모양 프록시를 통해 개발자는 해당 뷰의 속성을 사용자 지정할 수 있는 iOS의 이전 버전에서 도입 되었습니다. Ios 8 특성 컬렉션을 지원 하도록 확장 되었습니다.

모양 프록시에는 이제 새 메서드를 포함 `AppearanceForTraitCollection`, 전달 된 지정된 된 특성 컬렉션에 대 한 새로운 모양 프록시를 반환 하는입니다. 개발자는에 대해 수행 하는 모든 사용자 지정 모양을 프록시만 적용 됩니다 준수 하는 뷰에서 지정된 된 특성 컬렉션에 있습니다.

일반적으로 개발자는 부분적으로 지정 된 특성 컬렉션을 전달 합니다는 `AppearanceForTraitCollection` 지정는 가로 크기 클래스의를 압축은 가로 방향으로 압축 하는 응용 프로그램의 보기 중 하나를 사용자 지정할 수 있도록 하는 것과 같은 메서드.

## <a name="uiimage"></a>UIImage

Apple 추가 특성 컬렉션에는 다른 클래스 `UIImage`합니다. 이전에 지정 하는 개발자 했습니다는 @1X 및 @2x 버전 (예: 아이콘) 응용 프로그램에 포함을 비트맵 그래픽 자산은의 합니다.

iOS 8 개발자 특성 컬렉션을 기반으로 하는 이미지 카탈로그에 여러 버전의 이미지를 포함할 수 있도록 확장 되었습니다. 예를 들어 개발자는 Compact 특성 클래스 및 다른 컬렉션에 대 한 전체 크기의 이미지 작업에 대 한 더 작은 이미지를 포함 될 수 있습니다.

이미지 중 하나 사용 되는 경우 내부에 `UIImageView` 이미지 보기 클래스의 특성 컬렉션에 대 한 이미지의 올바른 버전을 자동으로 표시 됩니다. 이미지 보기를 새 특성 컬렉션을 일치 시키고 되는 이미지의 현재 버전의 일치 하도록 크기를 변경할 새 이미지 크기를 자동으로 선택 합니다 (예: 가로 세로에서 장치를 전환 사용자) 성분 환경 변경 되 면 표시 합니다.

## <a name="uiimageasset"></a>UIImageAsset

Apple이 라는 iOS 8에 새 클래스 추가 `UIImageAsset` 제어할 수 있도록 개발자 더 많은 이미지 선택 합니다.

이미지의 서로 다른 버전의 모든 이미지 자산을 래핑하고 개발자가 전달 하는 특성 컬렉션에 일치 하는 특정 이미지에 대 한 요청 수 있습니다. 이미지를 추가 하거나 이미지 자산에서 제거할 수 즉석에서.

이미지 자산에 대 한 자세한 내용은 Apple의을 참조 하십시오. [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) 설명서입니다.

## <a name="combining-trait-collections"></a>특성 컬렉션 결합

더하는 두은 개발자 특성 컬렉션에서 수행할 수 있는 또 다른 함수는 두 번째 식의 지정 된 값으로 하나의 컬렉션에서 지정 되지 않은 값의 바뀌는 결합된 된 컬렉션에서 발생 합니다. 이렇게 사용 하는 `FromTraitsFromCollections` 의 메서드는 `UITraitCollection` 클래스.

특성 컬렉션 중 하나에서 지정 되지 않은 특성 중 하나를 다른 지정 된 경우, 위에서 설명한 대로 값이 지정 된 버전으로 설정 됩니다. 그러나 지정 된 지정된 된 값의 여러 버전에 있는 경우 마지막에서 값 특성 컬렉션이 됩니다 사용 되는 값입니다.

## <a name="adaptive-view-controllers"></a>컨트롤러 적응 보기

이 섹션은 어떻게 iOS 뷰 및 컨트롤러 보기를 자동으로 개발자는 응용 프로그램에서 더 적응 특성 및 크기 클래스의 개념 채택한의 세부 정보를 설명 합니다.

### <a name="split-view-controller"></a>분할 뷰-컨트롤러

IOS 8에서 가장 많은 변경 된 뷰-컨트롤러 클래스 중 하나는 `UISplitViewController` 클래스입니다. 이전에 개발자 종종 응용 프로그램의 iPad 버전에서 분할 뷰 컨트롤러 사용 하며 다음 문서를 업로드 하려면 iPhone 버전의 앱에 대 한 계층 구조에 해당 보기의 완전히 다른 버전을 제공 합니다.

8, iOS에서의 `UISplitViewController` 클래스입니다. 두 플랫폼 모두 (iPad 및 iPhone)에서 사용할 수 있는 개발자는 iPhone 및 iPad를 둘 다에서 작동 하는 하나의 뷰-컨트롤러 계층 구조를 만들 수 있습니다.

가로 방향의 iPhone 이면 분할 뷰 컨트롤러는 iPad에서 표시 되는 경우와 마찬가지로 해당 뷰-side-by-side를 제공 합니다.

### <a name="overriding-traits"></a>특성을 재정의

자식 컨테이너 가로 방향의 iPad에서 분할 뷰 컨트롤러를 보여 주는 다음 그림에서와 같이 아래로 부모 컨테이너에서 성분 환경 계단식 배열:

 [![](unified-storyboards-images/cascadingclasses01.png "가로 방향의 iPad에서 분할 뷰 컨트롤러")](unified-storyboards-images/cascadingclasses01.png#lightbox)

IPad 가로 및 세로 방향에서 일반 크기 클래스에 있으므로 분할 보기에는 마스터 및 세부 정보 보기가 표시 됩니다.

여기서 크기 클래스는 두 방향 모두에서 압축, iPhone에서 분할 뷰 컨트롤러만 표시 세부 정보 보기 아래와 같이 합니다.

 [![](unified-storyboards-images/cascadingclasses02.png "분할 뷰 컨트롤러 세부 정보 보기 표시")](unified-storyboards-images/cascadingclasses02.png#lightbox)

응용 프로그램 개발자가 가로 방향의 iPhone에 마스터 및 세부 정보 보기를 표시 하려고 하는 위치에서 개발자는 분할 뷰 컨트롤러에 대 한 부모 컨테이너를 삽입 하 고 특성 컬렉션을 재정의 해야 합니다. 아래 그림에서와 같이:

 [![](unified-storyboards-images/cascadingclasses03.png "개발자는 분할 뷰 컨트롤러에 대 한 부모 컨테이너를 삽입 하 고 특성 컬렉션을 재정의 해야 합니다.")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A `UIView` 분할 뷰 컨트롤러의 부모로 설정 및 `SetOverrideTraitCollection` 보기에서 새 특성 컬렉션을 전달 하 고 분할 뷰 컨트롤러를 대상으로 메서드를 호출 합니다. 새 특성 컬렉션 재정의 `HorizontalSizeClass`로 설정 `Regular`에 분할 뷰 컨트롤러 가로 방향에 iPhone에서 마스터 및 세부 정보 보기를 표시 합니다.

사항에 유의 `VerticalSizeClass` 로 설정 된 `unspecified`, 결과적으로 부모 특성 컬렉션에 추가할 새 특성 컬렉션을 허용 하는 `Compact VerticalSizeClass` 자식 분할 뷰-컨트롤러에 대 한 합니다.

### <a name="trait-changes"></a>특성 변경

이 섹션에는 특성 컬렉션 특성 환경 변경 될 때 전환 하는 방법에 대해 자세히 설명 모양으로 걸립니다. 예를 들어 때 장치 회전 세로에서 가로로 합니다.

 [![](unified-storyboards-images/traittransitions01.png "특성 변경 내용 개요 가로 세로")](unified-storyboards-images/traittransitions01.png#lightbox)

첫째, iOS 8을 수행 하려면 전환에 대 한 준비 하려면 몇 가지 설치를 수행 합니다. 다음으로, 시스템 전환 상태에 애니메이션을 적용 합니다. 마지막으로 iOS 8 정리 전환 하는 동안 필요한 모든 임시 상태입니다.

iOS 8 개발자는 다음 표에 표시 된 대로 특성 변경에 참여 하는 데 사용할 수 있는 몇 가지 콜백을 제공 합니다.

|Phase|Callback|설명|
|--- |--- |--- |
|설정|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>이 메서드는 특성 컬렉션을 가져옵니다 새 값으로 설정 하기 전에 특성 변경의 시작 부분에서 호출 됩니다.</li><li>특성 컬렉션의 값이 변경 되었지만 애니메이션이 모두를 수행 하기 전에 메서드 호출을 가져옵니다.</li></ul>|
|애니메이션|`WillTransitionToTraitCollection`|이 메서드에 전달 되는 전환 코디네이터에는 `AnimateAlongside` 속성을 사용 하 고 개발자에 게 기본 애니메이션 함께 실행 될 애니메이션 추가 합니다.|
|정리|`WillTransitionToTraitCollection`|전환이 발생 한 후에 자신의 정리 코드를 포함 하는 개발자를 위한 방법을 제공 합니다.|

`WillTransitionToTraitCollection` 방법은 특성 컬렉션 변경 내용과 함께 컨트롤러 보기 애니메이션에 매우 유용 합니다. `WillTransitionToTraitCollection` 메서드는 컨트롤러 보기에서 사용할 수만 ( `UIViewController`) 특성의 다른 환경에서는 아닌 같은 `UIViews`합니다.

`TraitCollectionDidChange` 가 작업을 위한 높으면는 `UIView` 클래스, 특성을 변경 하는 대로 UI를 업데이트 하는 개발자가 하는 위치입니다.

### <a name="collapsing-the-split-view-controllers"></a>분할 뷰 컨트롤러 축소

이제 보겠습니다 take 어떤 자세히 보기에는 하나 이상의 열 보기에는 두 개의 열에서 분할 뷰 컨트롤러 축소 될 때 발생 합니다. 이 변경의 일부로 발생 해야 하는 두 프로세스를 가지 있습니다.

-  기본적으로 분할 뷰 컨트롤러는 기본 뷰 컨트롤러 보기로 사용할 축소 발생 한 후 합니다. 개발자를 재정의 하 여이 동작을 재정의할 수는 `GetPrimaryViewControllerForCollapsingSplitViewController` 의 메서드는 `UISplitViewControllerDelegate` 축소 된 상태로 표시 하고자 하는 모든 보기 컨트롤러를 제공 하 고 있습니다.
-  보조 뷰 컨트롤러는 주 보기 컨트롤러에 병합 합니다. 일반적으로 개발자는이 단계에 대 한 조치를 취할 필요가 없습니다. 분할 뷰 컨트롤러 하드웨어 장치에 따라이 단계의 자동 처리를 포함 합니다. 그러나 이러한 변경으로 인해 상호 작용 하는 개발자 풀려면 특별 한 경우도 있을 수 있습니다. 호출 된 `CollapseSecondViewController` 의 메서드는 `UISplitViewControllerDelegate` 마스터 뷰 컨트롤러가 세부 정보 보기 대신 축소 발생할 때 표시 될 수 있습니다.


### <a name="expanding-the-split-view-controller"></a>분할 뷰 컨트롤러 확장

이제 보겠습니다 take 어떤 자세히 보기 축소 된 상태에서 분할 뷰 컨트롤러 확장 될 때 발생 합니다. 다시 한 번 발생 해야 하는 두 단계로 가지가 있습니다.

-  새 기본 보기 컨트롤러를 먼저 정의 합니다. 분할 뷰 컨트롤러는 기본적으로 축소 된 보기에서 기본 보기 컨트롤러를 자동으로 사용 됩니다. 다시 사용 하 여이 문제는 개발자를 재정의할 수는 `GetPrimaryViewControllerForExpandingSplitViewController` 의 메서드는 `UISplitViewControllerDelegate` 합니다.
-  기본 보기 컨트롤러를 선택한 후 보조 뷰 컨트롤러를 다시 만들어야 합니다. 다시 분할 뷰 컨트롤러 하드웨어 장치에 따라이 단계의 자동 처리를 포함 합니다. 개발자는 호출 하 여이 동작을 재정의할 수는 `SeparateSecondaryViewController` 의 메서드는 `UISplitViewControllerDelegate` 합니다.


분할 뷰 컨트롤러의 기본 보기 컨트롤러 한 역할 확장와 구현 하 여 뷰의 축소 된 `CollapseSecondViewController` 및 `SeparateSecondaryViewController` 의 메서드는 `UISplitViewControllerDelegate`합니다. `UINavigationController` 이러한 메서드를 자동으로 푸시 및 팝 보조 뷰 컨트롤러를 구현 합니다.

### <a name="showing-view-controllers"></a>표시 된 컨트롤러 보기

Apple iOS 8 했습니다. 다른 변경 하는 개발자 컨트롤러 보기를 표시 하는 방식에서입니다. 응용 프로그램 (예: 테이블 뷰 컨트롤러), 리프 뷰 컨트롤러를 가지 며 개발자 표시 (예를 들어에 대 한 응답으로 셀에 사용자 탭), 다른 컨트롤러 계층을 통해 다시 응용 프로그램에 도달 하는 경우 이전에는 탐색 뷰-컨트롤러 및 호출 된 `PushViewController` 메서드를 표시할 새 보기에 대해 합니다.

이 탐색 컨트롤러에서 실행 중인 환경 간의 매우 밀접 한 결합을 제공 합니다. IOS 8에서에서 Apple에 분리이 두 개의 새 메서드를 제공 하 여:

-  `ShowViewController` – 해당 환경에 따라 새 뷰 컨트롤러 표시에 맞게 변경 합니다. 예를 들어 한 `UINavigationController` 스택에 새 보기를 단순히 푸시합니다. 분할 뷰 컨트롤러에서 새 기본 보기 컨트롤러로 새 뷰 컨트롤러 왼쪽에 표시 됩니다. 컨테이너 보기 컨트롤러가 없는 있는 경우 새 보기 모달 뷰 컨트롤러도 표시 됩니다.
-  `ShowDetailViewController` – 비슷한 방식으로 작동 `ShowViewController`, 하지만 자세히 보기에 전달 되 고 새 뷰 컨트롤러도 바꾸려면 분할 뷰 컨트롤러에서 구현 됩니다. 호출으로 리디렉션됩니다 (iPhone 응용 프로그램에서에서 볼 수)으로 분할 뷰 컨트롤러 축소 되어 있으면는 `ShowViewController` 메서드와 새 뷰의 기본 보기 컨트롤러로 표시 됩니다. 다시 컨테이너 보기 컨트롤러가 없는 있는 경우 새 보기는 모달 뷰 컨트롤러도 표시 됩니다.


이러한 메서드는 리프 뷰-컨트롤러부터 시작 하 여 작업 하 고 새 보기의 표시를 처리 하는 올바른 컨테이너 보기 컨트롤러를 찾을 때까지 뷰 계층 구조를 탐색 합니다.

개발자가 구현할 수 `ShowViewController` 및 `ShowDetailViewController` 기능을 자동화 된 동일한 가져오려는 자체 사용자 지정 보기의 컨트롤러에 있는 `UINavigationController` 및 `UISplitViewController` 제공 합니다.

### <a name="how-it-works"></a>작동 방법

이 섹션에서는 어떻게 이러한 메서드는 실제로 iOS 8에에서 구현 살펴보겠습니다 해당 메뉴로 이동 합니다. 첫 번째 새에서 살펴보겠습니다 `GetTargetForAction` 메서드:

 [![](unified-storyboards-images/gettargetforaction.png "새 GetTargetForAction 메서드")](unified-storyboards-images/gettargetforaction.png#lightbox)

이 메서드는 올바른 컨테이너 뷰-컨트롤러를 찾을 때까지 계층 구조 체인을 보여 줍니다. 예를 들어:

1.  경우는 `ShowViewController` 메서드가 호출 되 면이 메서드를 구현 하는 체인에서 첫 번째 뷰 컨트롤러 이므로 탐색 컨트롤러 새 보기의 부모로 사용 됩니다.
1.  경우는 `ShowDetailViewController` 분할 뷰 컨트롤러는의 부모로 사용 되도록 구현 하는 첫 번째 보기 컨트롤러, 메서드를 대신 호출 했습니다.


`GetTargetForAction` 방법은 지정된 된 동작을 구현 하는 뷰 컨트롤러를 찾아 다음 요청 하 여 해당 동작을 수신 하려는 경우 해당 뷰-컨트롤러입니다. 개발자 함수 처럼 기본 제공 하는 자체 사용자 지정 메서드를 만들 수 있습니다이 메서드는 공용 이므로 `ShowViewController` 및 `ShowDetailViewController` 메서드.

## <a name="adaptive-presentation"></a>적응 프레젠테이션

IOS 8에서에서 Apple Popover 프레젠테이션을 했습니다 ( `UIPopoverPresentationController`) 적응도 합니다. 되므로 Popover 프레젠테이션 뷰 컨트롤러 일반 크기 클래스에서 자동으로 기본 Popover 보기를 표시 합니다. 그러나이 전체 화면에에서 표시 가로로 압축 크기 클래스 (예: iPhone에서).

통합 된 스토리 보드 시스템 내에서 변경 내용에 맞추는, 새 컨트롤러 개체를 만든 발표 된 보기 컨트롤러 관리- `UIPresentationController`합니다. 이 컨트롤러 뷰 컨트롤러 해제 될 때까지 표시 되는 시간에서 만들어집니다. 관리 클래스와 그 간주할 수 슈퍼 클래스 뷰 컨트롤러를 통해 프레젠테이션 컨트롤러 제어는 됩니다 뷰 컨트롤러에 다시 제공 됩니다 (예: 방향) 뷰 컨트롤러에 영향을 주는 장치 변경에 응답 하는 동안 있습니다.

개발자는 뷰-컨트롤러를 사용 하 여 표시 되 면는 `PresentViewController` 메서드, 프레젠테이션 프로세스의 관리는 넘겨 `UIKit`합니다. UIKit 처리 (특히) 만들어지는 스타일에 대 한 올바른 컨트롤러, 되는 경우만 예외로 뷰 컨트롤러가로 설정 된 스타일 `UIModalPresentationCustom`합니다. 여기에서 응용 프로그램 고유의 PresentationController 대신이 사용 하 여 제공할 수는 `UIKit` 컨트롤러입니다.

### <a name="custom-presentation-styles"></a>사용자 지정 표시 스타일

사용자 지정 표시 스타일을 개발자가 사용자 지정 프레젠테이션 컨트롤러를 사용 하도록 선택할 수 있게 합니다. 이 사용자 지정 컨트롤러 모양 및 동작을 동맹은 뷰의 수정 데 사용할 수 있습니다.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>크기 클래스 사용

이 문서에 포함 되어 있는 적응 사진 Xamarin 프로젝트 크기 클래스와 적응 컨트롤러의 보기를 사용 하 여 iOS 8 통합 인터페이스 응용 프로그램에서 작업 예제를 제공 합니다.

응용 프로그램을 만들고 해당 UI 완전히 IOS 디자이너를 사용 하 여가 아닌 코드에서 하며 동일한 기술을 적용 통합 스토리 보드를 만드는 동안 이 문서의 뒷부분에 나오는 크기 클래스 통합 스토리 보드 및 iOS Xamarin 응용 프로그램에서 디자이너를 사용 하는 방법을 보여줍니다.

이제 좀 더 자세히 살펴보고 어떻게 적응 사진 프로젝트는 다양 한 크기의 클래스 기능에서에서 구현 iOS 8 적응 응용 프로그램을 만들어 보겠습니다.

### <a name="adapting-to-trait-environment-changes"></a>특성 환경 변경 내용에 맞게 조정

를 실행할 때 적응 사진 응용 프로그램을 사용 하는 iPhone에서 세로에서 가로로 장치를 회전할 때 분할 뷰 컨트롤러의 마스터 및 세부 정보 보기를 표시 됩니다.

 [![](unified-storyboards-images/rotation.png "분할 뷰 컨트롤러에는 모두 마스터 표시 되 고 그림과 같이 세부 정보 뷰")](unified-storyboards-images/rotation.png#lightbox)

재정의 하 여 이렇게는 `UpdateConstraintsForTraitCollection` 의 값을 기반으로 하는 뷰 컨트롤러 및 조정 하는 제약 조건에서 `VerticalSizeClass`합니다. 예를 들어:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>전환 애니메이션 추가

애니메이션 응용 프로그램에서 이동 적응 사진의 분할 뷰 컨트롤러에 확장 된 축소 하면 재정의 하 여 기본 애니메이션에 추가 됩니다는 `WillTransitionToTraitCollection` 보기 컨트롤러의 메서드. 예를 들어:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>특성 환경 재정의

위에 표시 된 것과 같이 적응 사진 응용 프로그램에서 강제로 모두 정보를 표시 하는 분할 뷰 컨트롤러와 마스터 뷰 iPhone 장치 가로 뷰에 있는 경우.

이 작업은 뷰 컨트롤러에서 다음 코드를 사용 하 여 수행 되었습니다.

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>확장 및 축소 분할 뷰 컨트롤러

다음에서는 Xamarin 분할 뷰 컨트롤러의 확장 및 축소 동작 기능이 구현 하는 방법에 대해 살펴보겠습니다. 에 `AppDelegate`분할 뷰 컨트롤러를 만들면, 이러한 변경 내용을 처리 하는 대리자를 할당 합니다.

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController` 메서드 테스트 사진 표시 되 고 해당 상태에 따라 작업을 수행 합니다. 사진이 추가 되지 경우 마스터 뷰 컨트롤러 표시 되도록 보조 뷰 컨트롤러 축소 것으로 표시 되 고 합니다.

`CollapseSecondViewController` 메서드 분할 뷰 컨트롤러를 확장할 때를 사용 하므로 해당 보기로 다시 축소 되는 경우 스택, 사진 모두 존재 하는 경우를 참조 하십시오.

### <a name="moving-between-view-controllers"></a>컨트롤러 보기 간에 이동

다음으로 적응 사진 응용 프로그램 컨트롤러 보기 간에 이동 하는 방법을에 대해 살펴보겠습니다. 에 `AAPLConversationViewController` 테이블에서 셀을 선택 하면 클래스는 `ShowDetailViewController` 메서드는 세부 정보 보기를 표시 하려면:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>공개 지표 표시

적응 사진 응용 프로그램에서 몇 가지 면 여기서 공개 표시기 숨겨져 있거나 성분 환경에 변경 내용을 기준으로 표시 합니다. 이 다음 코드로 처리 됩니다.

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

사용 하 여 구현 되며는 `GetTargetViewControllerForAction` 방법 위의 자세히 설명 합니다.

테이블 뷰 컨트롤러는 데이터를 표시할 때 여부 밀어넣기 수행 될, 및 표시 하거나 적절 하 게 공개 표시기를 숨길 것인지 여부를 보려면 위에서 구현 되는 메서드를 사용 합니다.

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>새 `ShowDetailTargetDidChangeNotification` 형식

Apple이 크기 클래스 및 분할 뷰 컨트롤러 내에서 특성 환경을 사용에 대 한 새 알림 유형을 추가 `ShowDetailTargetDidChangeNotification`합니다. 이 알림은 컨트롤러 확장 하거나 축소 하는 등 대상 분할 뷰 컨트롤러에 대 한 세부 정보 보기를 변경 될 때마다 전송 됩니다.

적응 사진 응용 프로그램이이 알림을 사용 하 여 세부 정보 뷰-컨트롤러는 변경 될 때 공개 표시기의 상태를 업데이트 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

특성 컬렉션 및 적응 컨트롤러 보기를 Xamarin.iOS에 통합 응용 프로그램을 만드는 쉽게 사용할 수 있습니다, 그리고 크기 클래스는 모든 방법을 보려면 적응 사진 응용 프로그램에 자세히 살펴보겠습니다.

## <a name="unified-storyboards"></a>통합 된 스토리 보드

새 스토리 보드 통합 하면 개발자가 만드는 하나, 여러 크기 클래스를 대상으로 하 여 iPhone 및 iPad를 둘 다 장치에 표시 될 수 있는 스토리 보드 파일 iOS 8에 통합 합니다. 통합 된 스토리 보드를 사용 하 여 개발자는 더 적은 UI 특정 코드 쓰고 만들고 유지 관리 하는 하나의 인터페이스 디자인 합니다.

스토리 보드 통합의 주요 이점은 다음과 같습니다.

-  IPhone 및 iPad에 대 한 동일한 스토리 보드 파일을 사용 합니다.
-  IOS 6 및 iOS 7 이전 버전인를 배포 합니다.
-  다양 한 장치, 방향 및 Xamarin iOS 디자이너 내에서 모든 OS 버전에 대 한 레이아웃을 미리 봅니다.

이 기능은 Mac Visual Studio에서 완전히 지원 됩니다.

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>크기 클래스를 사용 하도록 설정

기본적으로 모든 새 Xamarin.iOS 프로젝트는 클래스의 크기를 조정 합니다. 크기 클래스와 적응 Segues 내 이전 프로젝트에서 스토리 보드를 사용 하려면 먼저 변환 해야 iOS 디자이너 내에서 Xcode 6 통합 스토리 보드 형식입니다.

IOS 디자이너 및 검사에에서 변환에 스토리 보드를 열고이 작업을 수행 하는 **사용 크기 클래스** 확인란:

 [![](unified-storyboards-images/sizeclass01.png "크기 클래스를 사용 확인란")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS 디자이너는 크기 클래스를 사용 하는 스토리 보드의 형식을 변환 하려는 개발자는 확인:

 [![](unified-storyboards-images/sizeclass02.png "크기 클래스 경고 사용")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> 자동 레이아웃 제대로 작동 하려면 크기 클래스에 대 한 선택 해야 합니다.

### <a name="generic-device-types"></a>일반 장치 유형

스토리 보드 크기 클래스를 사용으로 변환 된, 일단 것은 될 다시 표시 디자인 화면에서 및 **다른 이름으로 뷰** 장치 제네릭 됩니다:

 [![](unified-storyboards-images/sizeclass03.png "일반 장치 유형으로 보기")](unified-storyboards-images/sizeclass03.png#lightbox)

일반 장치 유형을 선택 하면, 모든 컨트롤러 보기 600 x 600 사각형으로 크기가 조정 됩니다. 이 사각형 어떤 너비 및 높이 모든 크기를 나타냅니다. 이 모드에서는 iOS 디자이너 이면 편집 내용은 모든 크기 클래스에 적용 됩니다.

개발자는 iPhone로 디자인 화면을 표시 하는 옵션도 있습니다.

 [![](unified-storyboards-images/sizeclass04.png "IPhone로 디자인 화면을 보기")](unified-storyboards-images/sizeclass04.png#lightbox)

또는 iPad로 보기:

 [![](unified-storyboards-images/sizeclass05.png "IPad로 디자인 화면을 보기")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>크기 클래스 선택

디자인 화면으로 뷰 드롭다운) (근처의 왼쪽 위 모퉁이에 크기 클래스 선택기 단추가입니다. 현재 편집 중인 크기 클래스를 선택 하는 개발자 수 있습니다.

 [![](unified-storyboards-images/sizeclass06.png "크기 클래스 선택")](unified-storyboards-images/sizeclass06.png#lightbox)

선택기 크기 클래스 선택 3 x 3 그리드로 표시합니다. 각 표에 사각형 너비 클래스와 높이의 조합을 나타냅니다. 가운데 사각형 Any Width/Any 높이 크기 클래스 (즉, 통합 된 스토리 보드에 대 한 기본 뷰)를 선택 합니다. 이 사각형을 선택 하면 개발자가 다른 구성에 의해 상속 되는 기본 레이아웃을 편집 합니다.

모눈의 왼쪽 위 모퉁이에 있는 사각형 Compact 압축 너비/높이 크기 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass07.png "Compact 너비/압축 높이 크기 클래스")](unified-storyboards-images/sizeclass07.png#lightbox)

이 모드는 가로 방향의 iPhone에 해당합니다. 눈금의 오른쪽 아래 모퉁이에 있는 사각형 iPad를 나타내는 일반 일반 너비/높이 크기 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass08.png "일반 너비/일반 높이 크기 클래스")](unified-storyboards-images/sizeclass08.png#lightbox)

세로 방향의 iPhone에 대 한 레이아웃을 편집 하려면 왼쪽 아래 모서리에 있는 사각형을 선택 합니다. Compact 일반 너비/높이 크기 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass09.png "Compact 너비/일반 높이 크기 클래스")](unified-storyboards-images/sizeclass09.png#lightbox)

사각형 선택를 클릭 하 고 디자인 화면 보기 컨트롤러에서 새 선택 항목과 일치 하도록 크기를 변경 합니다.

 [![](unified-storyboards-images/sizeclass10.png "디자인 화면 보기 컨트롤러에서와 같이 새 선택 항목과 일치 하도록 크기를 변경 합니다.")](unified-storyboards-images/sizeclass10.png#lightbox)

크기 클래스 및 Iphone 및 Ipad에 대 한 레이아웃에 미치는 영향에 자세한 내용은이 문서의 크기 클래스 섹션을 참조 하십시오.

### <a name="adaptive-segue-types"></a>적응 Segue 형식

개발자가 이전에 스토리 보드를 사용 하는 경우 기존 segue 유형의 익숙할 것 **푸시**, **모달** 및 **Popover**합니다. 다음과 같은 적응 Segue (해당 하는 형식이 위에 설명 된 새 보기 컨트롤러 API)를 사용할 수 있도록 만들어진 크기 클래스 통합 스토리 보드 파일에 설정 되 면: **표시** 및 **세부 보기** .

> [!IMPORTANT]
> 크기 클래스 설정 된 경우에 segues 기존의 모든 새 형식으로 변환 합니다.

IOS의 예로 마스터 보기 간단한 게임 탐색 메뉴에 있는 분할 뷰 컨트롤러와 통합 된 스토리 보드를 사용 하는 8 응용 프로그램을 보겠습니다. 사용자가 메뉴 단추를 클릭 하면 선택한 항목의 뷰-컨트롤러 iPad에서 실행할 때 분할 뷰 컨트롤러의 세부 정보 섹션에 표시 합니다. IPhone에서 항목의 뷰-컨트롤러 탐색 스택에 밀어 넣은 해야 합니다.

이 효과 얻기 위해 iOS 디자이너에에서 컨트롤-단추를 클릭 하 고 표시할 보기 컨트롤러에 선을 끌어 합니다. 마우스 단추를 놓으면 선택 `Show Detail` Segue 형식 팝업 메뉴에서:

 [![](unified-storyboards-images/segue01.png "Segue 형식 팝업 메뉴에서 세부 보기를 선택 합니다.")](unified-storyboards-images/segue01.png#lightbox)

새 segue 단추와 뷰 컨트롤러 사이의 만들어질 수 있습니다. 이제는 iPhone 시뮬레이터에서에서 응용 프로그램을 실행 하 고 기본 메뉴가 표시 됩니다.

 [![](unified-storyboards-images/segue02.png "주 메뉴")](unified-storyboards-images/segue02.png#lightbox)

클릭는 **선택 게임** 단추와 해당 항목의 뷰-컨트롤러 탐색 스택에 밀어 넣은 됩니다.

 [![](unified-storyboards-images/segue03.png "항목 보기 컨트롤러 밀어넣습니다 탐색 스택에 표시 된 것 처럼")](unified-storyboards-images/segue03.png#lightbox)

IPhone 시뮬레이터를 중지 하 고 iPad 시뮬레이터에서에서 응용 프로그램을 실행 합니다. 주 가로 방향으로 전환 하 메뉴가 다시 표시 됩니다.

 [![](unified-storyboards-images/segue04.png "표시 되는 주 메뉴")](unified-storyboards-images/segue04.png#lightbox)

다시 클릭는 **선택 게임** 단추와 해당 항목의 뷰-컨트롤러 분할 뷰 컨트롤러의 세부 정보 섹션에 표시 됩니다.

 [![](unified-storyboards-images/segue05.png "뷰-컨트롤러 분할 뷰 컨트롤러의 세부 정보 섹션에 표시 된 항목")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>크기 클래스에서 요소 제외

지정된 된 요소 (예: 보기, 컨트롤 또는 제약 조건) 특정 크기 클래스 내부에서 필요 없는 경우 경우가 있습니다. 요소를 크기가 클래스에서 제외 하려면에서 제외 하려면 원하는 항목을 선택는 **디자인 화면**합니다. 아래쪽으로 스크롤하여는 **속성 탐색기** 클릭는 **기어** 드롭다운 메뉴. 조합을 선택 **너비** 및 **높이** 에서 항목을 제외 하려면:

[![](unified-storyboards-images/exclude-a.png "너비 및 높이의 조합을 선택합니다")](unified-storyboards-images/exclude-a.png#lightbox)

새 *제외 대/소문자* 의 맨 아래에 있는 요소에 추가 됩니다는 **속성 탐색기**합니다. 다음으로 선택 취소 된 **설치 됨** 주어진된 크기 클래스에 대 한 확인란:

[![](unified-storyboards-images/exclude-b.png "설치 된 확인란을 선택 취소")](unified-storyboards-images/exclude-b.png#lightbox)

디자인 화면 너비와 높이 항목에서 제외 된 전환, 지정된 된 크기 클래스 않고 전체 UI 디자인에서 제거 된:

 [![](unified-storyboards-images/exclude02.png "디자인 화면 너비와 높이 항목에서 제외 된 전환")](unified-storyboards-images/exclude02.png#lightbox)

Any Width/Any 높이 크기의 클래스 및 요소로 다시 전환 여전히 설정 되어 있습니다.

 [![](unified-storyboards-images/exclude03.png "Any Width/Any 높이 크기 클래스로 다시 전환")](unified-storyboards-images/exclude03.png#lightbox)

IPad 시뮬레이터에서에서 응용 프로그램이 실행 되는 요소는 나타납니다.

 [![](unified-storyboards-images/exclude04.png "표시 된 경우 요소 iPad 시뮬레이터에서에서 실행 중인 응용 프로그램")](unified-storyboards-images/exclude04.png#lightbox)

와 iPhone 시뮬레이터에서 응용 프로그램이 실행 되는 요소 없습니다.

 [![](unified-storyboards-images/exclude05.png "누락 된 경우 요소 iPhone 시뮬레이터에서에서 실행 중인 응용 프로그램")](unified-storyboards-images/exclude05.png#lightbox)

요소에서 제외 사례를 제거 하려면에서 요소를 선택 하기만 하면는 **디자인 화면**의 아래쪽으로 스크롤하여는 **속성 탐색기** 클릭는  **-** 제거 하는 경우 옆에 있는 단추입니다.

통합 된 스토리 보드의 구현을 보려면는 `UnifiedStoryboard` 샘플 Xamarin iOS 8 응용 프로그램에이 문서에 연결 합니다.

## <a name="dynamic-launch-screens"></a>동적 시작 화면

시작 화면 파일은 응용 프로그램은 실제로 시작 접속 하는 사용자에 게 피드백을 제공 하는 iOS 응용 프로그램을 시작할 동안 시작 화면으로 표시 됩니다. IOS 8 하기 전에 개발자에 여러 포함 해야 `Default.png` 자산에서 실행 되 고 응용 프로그램이 각 장치 유형, 방향 및 화면 해상도 대 한 이미지입니다. 예를 들어 `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`등입니다.

새 iPhone 6 및 iPhone 6 대비와 장치 (및 예정 된 Apple Watch) 모든 기존 iPhone 및 iPad 장치, 다양 한 크기, 방향 및 해상도의 대형 배열을 나타냅니다 `Default.png` 해야 하는 시작 화면 이미지 자산 생성 하 고 유지 관리 합니다. 또한 이러한 파일 매우 커질 수 있으며 됩니다 "팽창이" 결과물 응용 프로그램 번들을 iTunes App Store (가능 유지 셀룰러 네트워크를 통해 전달할 수 없습니다)에서 응용 프로그램을 다운로드 하는 데 필요한 시간을 증가 및 최종 사용자의 장치에 필요한 저장소의 양을 늘리십시오.

새로운 개발자 iOS 8에는 단일 원자 단위를 만들 수 `.xib` 자동 레이아웃 및 크기 클래스를 사용 하는 Xcode에서 파일을 *동적 시작 화면* 모든 장치, 해상도 및 방향에 대 한 작동 합니다. 이 뿐만 아니라 만들고 모든 필요한 이미지 자산, 유지 관리 하는 개발자는 데 필요한 작업 양을 줄어들지만 응용 프로그램의 설치 된 번들의 크기가 크게 줄어듭니다.


동적 시작 화면에는 다음과 같은 제한 사항 및 고려 사항 했습니다.

 - 만 사용 하 여 `UIKit` 클래스입니다.
 - 단일 루트 뷰를 사용 하 여 한 `UIView` 또는 `UIViewController` 개체입니다.
 - 응용 프로그램의 코드에 대 한 연결을 확인 하지 않습니다 (추가 하지 마십시오 **동작** 또는 **콘센트**).
 - 추가 하지 마십시오 `UIWebView` 개체입니다.
 - 모든 사용자 정의 클래스를 사용 하지 마십시오.
 - 런타임 특성을 사용 하지 마십시오.

염두에서 위의 지침을 살펴 보겠습니다 동적 시작 화면 기존 Xamarin iOS 8 프로젝트에 추가 합니다.

다음을 수행합니다.

1. 열기 **Mac 용 Visual Studio** 로드는 **솔루션** 동적 시작 화면을 추가 하려면.
2. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 `MainStoryboard.storyboard` 파일을 선택 **프로그램** > **Xcode 인터페이스 작성기**:

    [![](unified-storyboards-images/dls01.png "Xcode 인터페이스 작성기로 열기")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode에서 선택 **파일** > **새로** > **파일...** :

    [![](unified-storyboards-images/dls02.png "파일 / 새로 만들기")](unified-storyboards-images/dls02.png#lightbox)
4. 선택 **iOS** > **사용자 인터페이스** > **시작 화면** 클릭는 **다음** 단추:

    [![](unified-storyboards-images/dls03.png "IOS 선택 / 사용자 인터페이스 / 시작 화면")](unified-storyboards-images/dls03.png#lightbox)
5. 파일 이름을 `LaunchScreen.xib` 클릭는 **만들기** 단추:

    [![](unified-storyboards-images/dls04.png "LaunchScreen.xib 파일 이름")](unified-storyboards-images/dls04.png#lightbox)
6. 그래픽 요소를 추가 하 고 지정 된 장치, 방향 및 화면 크기에 대 한 위치로 레이아웃 제약 조건을 사용 하 여 실행 화면의 디자인을 편집 합니다.

    [![](unified-storyboards-images/dls05.png "편집 실행 화면 디자인")](unified-storyboards-images/dls05.png#lightbox)
7. 에 변경 내용을 저장 `LaunchScreen.xib`합니다.
8. 선택 된 **응용 프로그램 대상** 및 **일반** 탭:

    [![](unified-storyboards-images/dls06.png "일반 탭 및 응용 프로그램 대상 선택")](unified-storyboards-images/dls06.png#lightbox)
9. 클릭는 **선택 Info.plist** 단추를 선택는 `Info.plist` 누른 Xamarin 앱에 대 한는 **선택** 단추:

    [![](unified-storyboards-images/dls07.png "Xamarin 앱에 대 한 Info.plist 선택")](unified-storyboards-images/dls07.png#lightbox)
10. 에 **앱 아이콘 및 시작 이미지** 섹션에서 열고는 **시작 화면 파일** 드롭다운 선택는 `LaunchScreen.xib` 위에서 만든:

    [![](unified-storyboards-images/dls08.png "LaunchScreen.xib 선택")](unified-storyboards-images/dls08.png#lightbox)
11. 파일에 변경 내용을 저장 하 고 Mac.에 Visual Studio로 반환 합니다.
12. Visual Studio for Mac xcode 변경 내용을 동기화 완료 될 때까지 기다립니다.
13. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 **리소스** 폴더와 선택 **추가** > **파일 추가 중...** :

    [![](unified-storyboards-images/dls09.png "선택/추가 파일을 추가 중...")](unified-storyboards-images/dls09.png#lightbox)
14. 선택 된 `LaunchScreen.xib` 위에서 만든 파일을 클릭은 **열려** 단추:

    [![](unified-storyboards-images/dls10.png "LaunchScreen.xib 파일 선택")](unified-storyboards-images/dls10.png#lightbox)
15. 응용 프로그램을 빌드합니다.

### <a name="testing-the-dynamic-launch-screen"></a>동적 실행 화면 테스트

Mac 용 Visual Studio에서 iPhone 4 레 티 나 시뮬레이터를 선택 하 고 응용 프로그램을 실행 합니다. 동적 시작 화면에서 올바른 형식 및 방향을 표시 됩니다.

[![](unified-storyboards-images/dls11.png "세로 방향으로 표시 되는 동적 시작 화면")](unified-storyboards-images/dls11.png#lightbox)

Mac 용 Visual Studio에서 응용 프로그램을 중지 하 고 iOS 8 iPad 장치를 선택 합니다. 응용 프로그램을 실행 하 고 실행 화면 올바르게 포맷할이 장치와 방향에 대 한 키를 누릅니다.

[![](unified-storyboards-images/dls12.png "가로 방향으로 표시 되는 동적 시작 화면")](unified-storyboards-images/dls12.png#lightbox)

Mac 용 Visual Studio로 반환 하 고 응용 프로그램이 실행을 중지 합니다.

### <a name="working-with-ios-7"></a>IOS 7 작업

Ios 7 이전 버전과 호환성을 유지 하려면 포함는 일반적인 `Default.png` iOS 8 응용 프로그램에서 정상적으로 자산 이미지입니다. iOS 이전 동작으로 반환 하 고 iOS 7 장치에서 실행 하는 경우 시작 화면으로 해당 파일을 사용 합니다.

Xamarin에서 동적 시작 화면의 구현을 보려면는 [동적 시작 화면](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) 샘플이 문서에 연결 된 iOS 8 응용 프로그램입니다.

## <a name="summary"></a>요약

이 문서 크기 클래스와 iPhone 및 iPad 장치에서 레이아웃에 미치는 영향에 대해 간략히 살펴보고를 걸렸습니다. 해당 특성, 특성 환경 및 특성 컬렉션 된 통합 인터페이스를 만들 크기 클래스에서 작동 하는 방법을 설명 합니다. 적응 컨트롤러 보기에 대 한 간단한 설명 및 크기 통합 인터페이스는 클래스와 함께 작동 방법을 걸렸습니다. C# 코드 Xamarin iOS 8 응용 프로그램 내에서 크기 클래스와 통합 인터페이스를 완전히 구현에 검토 하는 것입니다.

마지막으로,이 문서에서는 Xamarin iOS는 iOS 장치에서 작동 하는 디자이너를 사용 하 여 통합 된 스토리 보드를 만드는 기본 사항 및 단일 동적 시작 화면을 만드는 모든 iOS 8 장치에서 시작 화면으로 표시 됩니다.

## <a name="related-links"></a>관련 링크

- [적응 사진 (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro 샘플](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [동적 시작 화면 (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [IOS8-발전 2014 (비디오)에서 동적 레이아웃](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
