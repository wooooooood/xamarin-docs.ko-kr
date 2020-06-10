---
title: Xamarin에서 tvOS 세그먼트화 된 컨트롤 사용
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 앱에서 tvOS 세그먼트화 된 컨트롤을 사용 하는 방법을 설명 합니다. 세그먼트 아이콘 및 텍스트, 이벤트, 컨트롤의 모양 수정 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: a31b0bcf3a61b5a1ea7e84f35131e6ceca1eef82
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569883"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Xamarin에서 tvOS 세그먼트화 된 컨트롤 사용

분할 된 컨트롤은 각각 아이콘이 나 텍스트를 포함할 수 있는 일련의 선형 요소를 제공 하며, 사용자에 게 관련 선택 항목 집합을 제공 하는 데 사용 됩니다.

[![](segmented-controls-images/segment01.png "Sample segment controls")](segmented-controls-images/segment01.png#lightbox)

Apple에는 분할 된 컨트롤을 사용 하기 위한 다음과 같은 제안이 있습니다.

- 다른 [포커스 가능 항목과](~/ios/tvos/app-fundamentals/navigation-focus.md) 분할 된 컨트롤 사이에 충분 한 공간을 제공 하려면 충분 한 **공간을 제공** 해야 합니다. 개별 세그먼트는 포커스가 있을 때 (클릭할 때가 아닌) 선택 되 고 사용자는 실제로 현재 세그먼트에서 포커스를 받을 수 있는 다른 항목을 선택 하려고 할 때 실수로 세그먼트를 변경할 수 있습니다.
- 콘텐츠 **필터링을 위한 분할 보기 사용** 분할 된 컨트롤은 콘텐츠와 필터를 쉽게 탐색할 수 있도록 디자인 되었으므로 분할 된 컨트롤을 사용 하 여 콘텐츠 필터링을 적절 하 게 선택 하지 않습니다.
- **최대 7 개의 세그먼트로 제한** -이는 소파에 있는 공간에서 더 쉽게 구문 분석 하 고 쉽게 탐색할 수 있으므로 최대 세그먼트 수를 8 (8) 미만으로 유지 해야 합니다.
- **일관성 있는 세그먼트 콘텐츠 사용** -모든 세그먼트의 너비가 같으며 가능 하면 각 세그먼트의 콘텐츠를 같은 크기로 유지 해야 합니다. 이렇게 하면 세그먼트 컨트롤이 시각적으로 보기 편 수 있을 뿐 아니라 한눈에 쉽게 읽을 수 있습니다.
- **아이콘과 텍스트를 혼합 하지 마세요** . 각 개별 세그먼트는 아이콘이 나 텍스트 중 하나만 포함할 수 있습니다. 동일한 분할 된 컨트롤에서 아이콘과 텍스트를 함께 사용할 수 있지만이는 피해 야 합니다.

<a name="About-Segment-Icons"></a>

## <a name="about-segment-icons"></a>세그먼트 아이콘 정보

Apple은 검색을 위한 돋보기와 같이 세그먼트 아이콘에 대해 간단 하 고 인식 가능한 이미지를 사용 하는 것을 제안 합니다. 너무 복잡 한 아이콘은 실내에서 TV 화면을 인식 하기 어려울 수 있으므로 아이콘을 간단한 표현으로 제한 하는 것이 가장 좋습니다.

지정 된 세그먼트에서 텍스트와 아이콘을 혼합할 수 없으며 분할 된 단일 컨트롤에서 아이콘과 텍스트를 혼합 하지 않아야 합니다. 모든 아이콘 또는 모든 텍스트 여야 합니다.

<a name="Summary"></a>

## <a name="segment-text"></a>세그먼트 텍스트

Apple은 세그먼트 텍스트 작업에 대해 다음과 같은 제안을 합니다.

- **의미 있는 단기 명사를 사용** 합니다. 세그먼트 제목은 지정 된 세그먼트를 선택할 때 사용자가 필요로 하는 콘텐츠 형식을 명확 하 게 명시 해야 합니다. 예: 음악 또는 동영상.
- **제목-대/소문자 표기 사용** -세그먼트 제목의 모든 단어는 4 자 미만의 접속사 및 전치사 문서를 제외 하 고 대문자로 표시 되어야 합니다.
- **짧은 중심의 제목을 사용** 합니다. 즉, 세그먼트를 선택할 때 사용할 콘텐츠 형식에 대 한 제목을 짧게 유지 합니다.

또한 지정 된 세그먼트에서 텍스트와 아이콘을 혼합할 수 없으며 단일 분할 된 컨트롤에서 아이콘과 텍스트를 혼합 하지 않아야 합니다.

<a name="Segment-Controls-and-Storyboards"></a>

## <a name="segment-controls-and-storyboards"></a>세그먼트 컨트롤 및 Storyboard

TvOS 앱에서 세그먼트 컨트롤로 작업 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. **Solution Pad**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **세그먼트 컨트롤** 을 끌어 뷰에 놓습니다. 

    [![](segmented-controls-images/segment02.png "A Segment Control")](segmented-controls-images/segment02.png#lightbox)
1. **속성 패드**의 **위젯 탭** 에서 해당 **스타일** 및 **상태**와 같은 세그먼트 컨트롤의 여러 속성을 조정할 수 있습니다. 

    [![](segmented-controls-images/segment03.png "The Widget Tab")](segmented-controls-images/segment03.png#lightbox)
1. **세그먼트** 필드를 사용 하 여 컨트롤러의 세그먼트 수를 제어 합니다.
1. **세그먼트 드롭다운에서** 지정 된 세그먼트를 선택 하 여 **제목** 또는 **이미지** 와 같은 개별 속성을 조정 하 고, 컨트롤이 표시 될 때 지정 된 세그먼트가 **활성화** 또는 **선택** 되는지 여부를 제어 합니다.
1. 마지막으로 c # 코드에서이에 응답할 수 있도록 컨트롤에 **이름을** 할당 합니다. 예를 들면 다음과 같습니다. 

    [![](segmented-controls-images/segment04.png "Assign a Name")](segmented-controls-images/segment04.png#lightbox)
1. 변경 내용을 저장합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **세그먼트 컨트롤** 을 끌어 뷰에 놓습니다. 

    [![](segmented-controls-images/segment02-vs.png "A Segment Control")](segmented-controls-images/segment02-vs.png#lightbox)
1. **속성 탐색기**의 **위젯 탭** 에서 해당 **스타일** 및 **상태**와 같은 세그먼트 컨트롤의 여러 속성을 조정할 수 있습니다. 

    [![](segmented-controls-images/segment03-vs.png "The Widget Tab")](segmented-controls-images/segment03-vs.png#lightbox)
1. **세그먼트** 필드를 사용 하 여 컨트롤러의 세그먼트 수를 제어 합니다.
1. **세그먼트 드롭다운에서** 지정 된 세그먼트를 선택 하 여 **제목** 또는 **이미지** 와 같은 개별 속성을 조정 하 고, 컨트롤이 표시 될 때 지정 된 세그먼트가 **활성화** 또는 **선택** 되는지 여부를 제어 합니다.
1. 마지막으로 c # 코드에서이에 응답할 수 있도록 컨트롤에 **이름을** 할당 합니다. 예를 들면 다음과 같습니다. 

    [![](segmented-controls-images/segment04-vs.png "Assign a Name")](segmented-controls-images/segment04-vs.png#lightbox)
1. 변경 내용을 저장합니다.

-----

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요. 

<a name="Working-with-Segmented-Controls"></a>

## <a name="working-with-segmented-controls"></a>분할 된 컨트롤 작업

위에서 설명한 것 처럼 분할 된 컨트롤은 각각 아이콘이 나 텍스트를 포함할 수 있는 일련의 선형 요소를 제공 하며, 사용자에 게 관련 선택 항목 집합을 제공 하는 데 사용 됩니다.

TvOS 앱에서 세그먼트화 된 컨트롤을 사용할 수 있는 여러 가지 방법이 있습니다.

<a name="Exposed-as-Outlets-and-Actions"></a>

## <a name="exposed-as-names-and-events"></a>이름 및 이벤트로 노출 됨

인터페이스 디자이너에서 세그먼트 컨트롤을 만들고이를 명명 된 컨트롤 및 이벤트로 노출 한 경우 다음 코드를 사용 하 여 변경 된 세그먼트에 응답할 수 있습니다.

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

위 예제의 경우 세그먼트 컨트롤이 `PlayerCount` 이름 및 이벤트 작업으로 노출 되었습니다 `PlayerCountChanged` . 작업 및 콘센트 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)의 [콘센트 및 작업을 사용 하 여 코드 작성](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) 섹션을 참조 하세요.

`SelectedSegment`속성은 현재 선택한 세그먼트를 0부터 시작 하는 인덱스를 가져오거나 설정 합니다. 따라서 세그먼트가 5 개 있는 경우 첫 번째 세그먼트의 인덱스는 0 (0)이 고 마지막 인덱스는 4 (4)입니다.

<a name="Modifying-Segments"></a>

## <a name="modifying-segments"></a>세그먼트 수정

언제 든 지 분할 된 컨트롤의 수와 콘텐츠를 수정할 수 있습니다. 새 아이콘 세그먼트를 삽입 하려면 다음 코드를 사용 합니다.

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

두 번째 매개 변수는 0부터 시작 하는 인덱스를 사용 하 여 세그먼트가 삽입 되는 위치를 정의 합니다. 마지막 매개 변수가 이면 `true` 삽입에 애니메이션을 적용 합니다.

지정 된 세그먼트를 제거 하려면 다음을 사용 합니다.

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

모든 세그먼트를 제거 하려면 다음을 수행 합니다.

```csharp
SegmentedControl.RemoveAllSegments();
```

마지막 매개 변수가 이면 `true` 제거에 애니메이션 효과가 적용 됩니다. `NumberOfSegments`현재 세그먼트 수를 반환 하려면 속성을 사용 합니다.

지정 된 세그먼트의 **제목** 또는 **아이콘** 을 가져오려면 다음을 사용 합니다.

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

**제목** 또는 **아이콘**을 변경 하려면 다음을 사용 합니다.

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

지정 된 세그먼트가 **사용 되는지 확인**하려면 다음을 사용 합니다.

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

지정 된 세그먼트를 **설정/해제** 하려면 다음을 사용 합니다.

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance"></a>

## <a name="modifying-the-segmented-controls-appearance"></a>분할 된 컨트롤의 모양 수정

다음 코드를 사용 하 여 지정 된 세그먼트의 배경을 이미지로 변경할 수 있습니다.

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

여기서는 `UIControlState` 이미지를 설정 하는 컨트롤의 상태를 다음과 같이 지정 합니다.

- 정상
- 강조 표시됨
- 사용 안 함
- 선택함
- 포커스 있음

그리고 `UIBarMetrics` 다음과 같이 사용할 메트릭을 지정 합니다.

- Default
- 컴팩트
- DefaultPrompt
- CompactPrompt

또한 다음을 사용 하 여 세그먼트 간 구분선을 설정할 수 있습니다.

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

여기서 첫 번째는 `UIControlState` 분할선의 왼쪽에 있는 세그먼트의 상태를 지정 하 고 두 번째는 `UIControlState` 세그먼트의 상태를 오른쪽으로 지정 합니다.

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내부에서 분할 된 컨트롤을 디자인 하 고 작업 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
