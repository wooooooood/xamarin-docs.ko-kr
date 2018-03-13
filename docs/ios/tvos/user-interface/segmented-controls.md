---
title: "분할 된 컨트롤을 사용한 작업"
description: "이 문서에서는 디자인 하 고 세그먼트 컨트롤 Xamarin.tvOS 앱 내에서 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 6561ff70997af05ed4df6b7bfe0ba6345fb44d9d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-segmented-controls"></a>분할 된 컨트롤을 사용한 작업

_이 문서에서는 디자인 하 고 세그먼트 컨트롤 Xamarin.tvOS 앱 내에서 작업을 설명 합니다._


분할 된 컨트롤 아이콘 또는 텍스트를 포함할 수 있습니다 및 관련 된 선택 항목 집합이 사용자에 게 제공 하는 데 사용 되는 각각의 선형 요소 집합을 제공 합니다.

[![](segmented-controls-images/segment01.png "샘플 세그먼트 컨트롤")](segmented-controls-images/segment01.png#lightbox)

Apple에 컨트롤 세그먼트 작업을 위한 다음 제안 사항이 있습니다.

- **충분 한 공간을 제공** -보험 다른 간의 충분 한 공간을 제공 하는 데 걸리는 해야 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 세그먼트 제어 합니다. 개별 세그먼트 (하지 클릭할 때) 포커스 되 고 실제로 현재 세그먼트에 포커스를 받을 수 다른 항목을 선택 하려는 경우 사용자 세그먼트를 실수로 변경 수 선택 됩니다.
- **분할 뷰를 사용 하 여 콘텐츠 필터링에 대 한** -컨트롤 세그먼트 콘텐츠 분할 뷰 내용과 필터 사이 쉽게 탐색을 위해 설계 된 대로 필터링에 대 한 좋은 선택 만들지 못합니다.
- **세그먼트가 최대 7 개에 제한이** -8 아래 세그먼트의 최대 수 (8)로 유지 하려고 노력 해야 간에 소파 쉽고 탐색에서 구문 분석을이 더 쉽습니다.
- **일관 된 세그먼트 콘텐츠 크기를 사용 하 여** -모든 세그먼트 너비를 같게 만들기 있고, 가능한 경우 같은 크기 각 세그먼트에서 콘텐츠를 유지 하려고 해야 합니다. 세그먼트 컨트롤을 시각적으로 만족 하면 뿐만 아니라 쉽게를 한 눈에 읽을 수 있습니다.
- **혼합 아이콘 및 텍스트 방지** -각 개별 세그먼트 아이콘 또는 텍스트 중 하나만 포함 될 수 있습니다. 아이콘과 같은 세그먼트화 된 컨트롤의에서 텍스트를 혼합 하는 가능 하지만이 피해 야 합니다.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>세그먼트 아이콘에 대 한

Apple 간단 하 고 인식할 수 있는 이미지를 사용 하 여 검색을 위한 돋보기 등 세그먼트 아이콘에 대 한 제안 합니다. 지나치게 복잡 한 아이콘은 어렵습니다 인식 TV 화면의 대화방에 걸쳐 하므로 간단한 표현에 아이콘이 제한 하는 것이 좋습니다.

텍스트와 지정된 된 세그먼트에 있는 아이콘을 혼합할 수 없습니다 및 아이콘 및 단일 분할 된 컨트롤의에서 텍스트를 혼합 하지 않아야 합니다. 모든 아이콘 또는 모든 텍스트 여야 합니다.

<a name="Summary" />

## <a name="segment-text"></a>세그먼트 텍스트

Apple에서는 세그먼트 텍스트 작업에 대 한 다음 제안 합니다.

- **즉, 의미 있는 명사를 사용 하 여** -The 세그먼트 제목 사용자 지정된 세그먼트를 선택할 때 필요로 하는 콘텐츠의 형식을 명확 하 게 명시 합니다. 예를 들어: 음악 또는 비디오.
- **제목 대/소문자 대/소문자를 사용 하 여** -접속사 아티클과 전치사 보다 작거나 4 개의 문자를 제외 하 고 세그먼트 타이틀의 모든 단어는 대문자로 시작 해야 합니다.
- **즉, 포커스가 있는 제목을 사용 하 여** -짧고 기대 세그먼트를 선택 하는 콘텐츠 형식의에 초점을 맞추고 호칭을 유지 합니다.

다시, 텍스트와 지정된 된 세그먼트에 있는 아이콘을 혼합할 수 없습니다 고 아이콘 및 단일 분할 된 컨트롤의에서 텍스트를 혼합 하지 않아야 합니다.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>스토리 보드와 세그먼트 컨트롤

Xamarin.tvOS 응용 프로그램에서 세그먼트 컨트롤을 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 에 **솔루션 패드**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **세그먼트 제어** 에서 **도구 상자** 보기에 놓습니다. 

    [![](segmented-controls-images/segment02.png "세그먼트 컨트롤")](segmented-controls-images/segment02.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 패드**와 같은 세그먼트 컨트롤의 몇 가지 속성을 조정할 수 있습니다는 **스타일** 및 **상태**: 

    [![](segmented-controls-images/segment03.png "위젯 탭")](segmented-controls-images/segment03.png#lightbox)
1. 사용 하 여는 **세그먼트** 컨트롤러에서 세그먼트의 수를 제어 하는 필드입니다.
1. 지정된 된 세그먼트를 선택는 **세그먼트 드롭다운** 와 같은 개별 속성을 조정 하려면 **제목** 또는 **이미지** 을 지정 된 세그먼트가 제어  **활성화** 또는 **선택한** 컨트롤을 표시 합니다.
1. 마지막으로 할당 **이름** 컨트롤에 C# 코드에서에 응답할 수 있도록 합니다. 예: 

    [![](segmented-controls-images/segment04.png "이름을 할당합니다")](segmented-controls-images/segment04.png#lightbox)
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **세그먼트 제어** 에서 **도구 상자** 보기에 놓습니다. 

    [![](segmented-controls-images/segment02-vs.png "세그먼트 컨트롤")](segmented-controls-images/segment02-vs.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 탐색기**와 같은 세그먼트 컨트롤의 몇 가지 속성을 조정할 수 있습니다는 **스타일** 및 **상태**: 

    [![](segmented-controls-images/segment03-vs.png "위젯 탭")](segmented-controls-images/segment03-vs.png#lightbox)
1. 사용 하 여는 **세그먼트** 컨트롤러에서 세그먼트의 수를 제어 하는 필드입니다.
1. 지정된 된 세그먼트를 선택는 **세그먼트 드롭다운** 와 같은 개별 속성을 조정 하려면 **제목** 또는 **이미지** 을 지정 된 세그먼트가 제어  **활성화** 또는 **선택한** 컨트롤을 표시 합니다.
1. 마지막으로 할당 **이름** 컨트롤에 C# 코드에서에 응답할 수 있도록 합니다. 예: 

    [![](segmented-controls-images/segment04-vs.png "이름을 할당합니다")](segmented-controls-images/segment04-vs.png#lightbox)
1. 변경 내용을 저장합니다.
    
-----

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>분할 된 컨트롤을 사용한 작업

조각화 된 컨트롤은 선형 요소 집합이 제공 s 위에서 설명한 대로 아이콘 또는 텍스트를 포함할 수 있습니다는 각각 및 관련 된 선택 항목 집합이 사용자에 게 제공 하는 데 사용 됩니다.

다양 한 방법으로 Xamarin.tvOS 응용 프로그램에서 조각화 된 컨트롤을 사용할 수 있습니다.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>이름 및 이벤트로 노출

세그먼트 컨트롤 인터페이스 디자이너에서 만든 명명 된 제어 및 이벤트로 노출 하는 경우에 세그먼트 변경에 응답 하는 다음 코드를 사용할 수 있습니다.

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

위 예의 경우 세그먼트 컨트롤으로 노출 된는 `PlayerCount` 이름 및 `PlayerCountChanged` 이벤트 동작 합니다. 작업 동작 및 콘센트에 대 한 자세한 내용은 참조 하십시오는 [콘센트 및 동작을 사용 하 여 코드를 작성](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) 섹션 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

`SelectedSegment` 속성 기반 영 (0)와 현재 선택 된 세그먼트 인덱스를 가져오거나 설정 합니다. 5 개의 세그먼트를 설정한 경우 첫 번째 세그먼트를 가집니다 영 (0) 및 마지막 인덱스를 인덱스의 4 개 (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>세그먼트 수정

언제 든 지 수 뷰와 세그먼트화 된 컨트롤의 콘텐츠를 수정할 수 있습니다. 다음 코드를 사용 하 여 새 아이콘 세그먼트를 삽입 하려면:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

두 번째 매개 변수 정의 세그먼트 될 위치는 0 개 (0) 시작된 하는 인덱스를 사용 하 여 삽입 합니다. 마지막 매개 변수가 `true` 삽입 애니메이션 효과가 적용 됩니다.

지정된 된 세그먼트를 제거 하려면 다음 절차를 사용 합니다.

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

또는 다음 모든 세그먼트를 제거 하려면:

```csharp
SegmentedControl.RemoveAllSegments();
```

다시, 마지막 매개 변수가 `true`, 제거 애니메이션 효과가 적용 됩니다. 사용 하 여는 `NumberOfSegments` 세그먼트의 현재 수를 반환 하는 속성입니다.

가져오려는 **제목** 또는 **아이콘** 지정된 된 세그먼트에 대 한 다음을 사용 합니다.

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

변경할 수는 **제목** 또는 **아이콘**, 다음을 사용 합니다.

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

지정된 된 세그먼트 인지 **Enabled**, 다음을 사용 하 여:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

및 **설정/해제** 는 세그먼트를 사용 하 여 다음:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>분할 된 컨트롤의 모양 수정

이미지에 지정된 된 세그먼트의 배경을 변경 하려면 다음 코드를 사용할 수 있습니다.

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

여기서 `UIControlState` 으로 이미지를 설정 하는 컨트롤의 상태를 지정 합니다.

- 보통
- 강조 표시
- 사용 안 함
- 선택함
- 포커스 있음

및 `UIBarMetrics` 으로 사용 하는 메트릭을 지정 합니다.

- 기본
- 압축
- DefaultPrompt
- CompactPrompt

또한 사용 하 여 세그먼트 사이의 구분선을 설정할 수 있습니다.

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

여기서는 첫 번째 `UIControlState` 구분선과 두 번째의 왼쪽에는 세그먼트의 상태를 지정 `UIControlState` 오른쪽 세그먼트의 상태를 지정 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 하 고 Xamarin.tvOS 앱 내에서 조각화 된 제어를 사용한 작업 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
