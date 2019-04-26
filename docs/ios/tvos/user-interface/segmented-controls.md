---
title: Xamarin에서 tvOS 분할 된 컨트롤을 사용 하 여 작업
description: 이 문서에서는 Xamarin을 사용 하 여 빌드한 앱에서 분할 된 컨트롤 tvOS를 사용 하는 방법을 설명 합니다. 세그먼트 아이콘 및 텍스트, 컨트롤의 모양 및 기타 정보를 수정 하는 이벤트에 설명 합니다.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 98a770d05014e0498b805ed9ffa0c84314efc765
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61375041"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Xamarin에서 tvOS 분할 된 컨트롤을 사용 하 여 작업

요소의 선형 아이콘 또는 텍스트를 포함할 수 있습니다 및 사용자에 게 관련 된 선택 항목 집합을 제공 하는 데 사용 되는 각 분할 된 제어를 제공 합니다.

[![](segmented-controls-images/segment01.png "샘플 세그먼트 컨트롤")](segmented-controls-images/segment01.png#lightbox)

Apple에 분할 된 컨트롤을 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **충분 한 공간을 제공** -주의 다른 간에 충분 한 공간을 제공 하는 데 걸리는 해야 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 분할 된 제어를 합니다. 개별 세그먼트를 포커스 내 (없습니다 클릭할 때) 이므로 실제로 현재 세그먼트에 포커스를 받을 수 다른 항목을 선택 하려고 할 때 사용자 세그먼트를 실수로 변경할 수 있습니다 하는 경우에 선택 됩니다.
- **분할 뷰를 사용 하 여 콘텐츠 필터링** -분할 된 컨트롤 분할 뷰 필터와 콘텐츠 사이의 쉽게 탐색 용으로 설계 된 대로 필터링 하는 콘텐츠에 대 한 좋은 선택을 하지 않습니다.
- **7 세그먼트 최대 제한** -8 아래 세그먼트의 최대 수 (8)으로 유지 하도록 노력 해야 이동할 소파 쉽고 대화방에서에서 구문 분석에이 더 쉽습니다.
- **일관 된 세그먼트 콘텐츠 크기를 사용 하 여** -모든 세그먼트가 동일한 너비로 있고, 가능한 경우 동일한 크기 각 세그먼트에서 콘텐츠를 유지 하려고 해야 합니다. 이 세그먼트 컨트롤을 시각적으로 맞출 수 있습니다 뿐만 아니라 쉽게 한눈에 읽을 수 있도록 합니다.
- **혼합 아이콘 및 텍스트 방지** -각 개별 세그먼트 아이콘 또는 텍스트 중 하나만 포함 될 수 있습니다. 아이콘과 같은 분할 된 컨트롤의 텍스트를 모두 함께 사용할 수 있지만,이 피해 야 합니다.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>세그먼트 아이콘에 대 한

Apple에서 간단 하 고 인식할 수 있는 이미지를 사용 하 여 검색을 위해 돋보기 같은 세그먼트 아이콘에 대 한 제안 합니다. 지나치게 복잡 한 아이콘은 간단한 표현에 아이콘을 제한 하는 것이 가장 좋습니다 대화방에 걸쳐 TV 화면의 인식 어렵습니다.

텍스트와 지정된 된 세그먼트에 있는 아이콘을 혼합할 수 없습니다 하 고 아이콘 및 단일 분할 된 컨트롤의 텍스트를 혼합 하지 마십시오. 모든 아이콘 또는 모든 텍스트 여야 합니다.

<a name="Summary" />

## <a name="segment-text"></a>세그먼트 텍스트

Apple에서는 세그먼트 텍스트를 사용 하 여 작업 하기 위한 다음 제안 합니다.

- **짧은, 의미 있는 명사를 사용 하 여** -The 세그먼트 제목 사용자 지정된 세그먼트를 선택할 때 필요로 하는 콘텐츠 형식의 상태 명확 하 게 해야 합니다. 예를 들어: 음악 또는 비디오를 제공 합니다.
- **첫 글자를 대문자로 대/소문자를 사용 하 여** -문서, 결합 및 전치사 보다 작은 4 개의 문자를 제외 하 고 세그먼트의 모든 단어는 대문자로 시작 해야 합니다.
- **짧은 초점을 맞춘 제목을 사용 하 여** -단기 및 세그먼트는 선택 될 때 예상 되는 콘텐츠 형식에 포커스가 있는 제목, 유지 합니다.

마찬가지로 텍스트와 지정된 된 세그먼트에 있는 아이콘을 혼합할 수 없습니다 및 아이콘 및 단일 분할 된 컨트롤의 텍스트를 혼합 하지 마십시오.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>컨트롤 세그먼트 및 스토리 보드

Xamarin.tvOS 앱에서 세그먼트 컨트롤을 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가할 경우

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **세그먼트 컨트롤** 에서 **도구 상자** 보기에 놓습니다. 

    [![](segmented-controls-images/segment02.png "세그먼트 컨트롤")](segmented-controls-images/segment02.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 패드**, 같은 세그먼트 컨트롤의 여러 속성을 조정할 수 있습니다 해당 **스타일** 하 고 **상태**: 

    [![](segmented-controls-images/segment03.png "위젯 탭")](segmented-controls-images/segment03.png#lightbox)
1. 사용 된 **세그먼트** 컨트롤러에서 세그먼트의 수를 제어 하는 필드입니다.
1. 지정된 된 세그먼트를 선택 합니다 **세그먼트 드롭다운** 와 같은 개별 속성을 조정 하 **Title** 또는 **이미지** 하는 경우 지정된 된 세그먼트의 컨트롤  **사용 하도록 설정** 나 **선택한** 컨트롤을 표시 하는 경우.
1. 마지막으로 할당 **이름을** 컨트롤에 대응할 수 있도록 C# 코드입니다. 예를 들어: 

    [![](segmented-controls-images/segment04.png "이름을 할당합니다")](segmented-controls-images/segment04.png#lightbox)
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 에 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **세그먼트 컨트롤** 에서 **도구 상자** 보기에 놓습니다. 

    [![](segmented-controls-images/segment02-vs.png "세그먼트 컨트롤")](segmented-controls-images/segment02-vs.png#lightbox)
1. 에 **위젯을 탭** 의 합니다 **속성 탐색기**와 같은 세그먼트 컨트롤의 여러 속성을 조정할 수 있습니다 해당 **스타일** 및 **상태**: 

    [![](segmented-controls-images/segment03-vs.png "위젯 탭")](segmented-controls-images/segment03-vs.png#lightbox)
1. 사용 된 **세그먼트** 컨트롤러에서 세그먼트의 수를 제어 하는 필드입니다.
1. 지정된 된 세그먼트를 선택 합니다 **세그먼트 드롭다운** 와 같은 개별 속성을 조정 하 **Title** 또는 **이미지** 하는 경우 지정된 된 세그먼트의 컨트롤  **사용 하도록 설정** 나 **선택한** 컨트롤을 표시 하는 경우.
1. 마지막으로 할당 **이름을** 컨트롤에 대응할 수 있도록 C# 코드입니다. 예를 들어: 

    [![](segmented-controls-images/segment04-vs.png "이름을 할당합니다")](segmented-controls-images/segment04-vs.png#lightbox)
1. 변경 내용을 저장합니다.
    
-----

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>분할 된 컨트롤 사용

선형 요소 집합을 제공 하는 분할 된 제어 s 위에서 설명한 대로 아이콘 또는 텍스트를 포함할 수 있습니다 각 및 사용자에 게 관련 된 선택 항목 집합을 제공 하는 데 사용 됩니다.

Xamarin.tvOS 앱에서 분할 된 컨트롤을 사용 하 여 사용할 수 있는 여러 가지가 있습니다.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>이름 및 이벤트로 노출합니다.

인터페이스 디자이너에서 세그먼트 컨트롤을 만들고 명명 된 컨트롤 및 이벤트로 노출 하는 경우 다음 코드 세그먼트 변화에 사용할 수 있습니다.

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

위의 예에서 세그먼트 컨트롤으로 노출 된를 `PlayerCount` 이름 및 `PlayerCountChanged` 이벤트 동작 합니다. 작업 및 출 선 작업에 대 한 자세한 내용은 참조 하십시오 합니다 [출 선 및 작업을 사용 하 여 코드를 작성](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) 부분 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

`SelectedSegment` 속성 기반 영 (0)으로 현재 선택한 세그먼트 인덱스를 가져오거나 설정 합니다. 갖도록 5 (5) 세그먼트에 있는 경우 첫 번째 세그먼트는 영 (0) 및 마지막 인덱스의 인덱스 4 (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>세그먼트 수정

언제 든 지 수와 분할 된 컨트롤의 콘텐츠를 수정할 수 있습니다. 다음 코드를 사용 하 여 세그먼트를 새 아이콘을 삽입 합니다.

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

두 번째 매개 변수 정의 세그먼트 될 위치 0 0 기반된 인덱스를 사용 하 여 삽입 합니다. 마지막 매개 변수인 경우 `true` 삽입 애니메이션 효과가 적용 됩니다.

지정된 된 세그먼트를 제거 하려면 다음을 사용 합니다.

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

또는 다음 모든 세그먼트를 제거 하려면:

```csharp
SegmentedControl.RemoveAllSegments();
```

마찬가지로 마지막 매개 변수인 경우 `true`, 제거 애니메이션 효과가 적용 됩니다. 사용 된 `NumberOfSegments` 세그먼트의 현재 수를 반환 하는 속성입니다.

가져오려는 합니다 **제목** 하거나 **아이콘** 지정된 된 세그먼트에 대 한 다음 사용 하 여:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

변경 하는 **제목** 하거나 **아이콘**, 다음 사용 하 여:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

지정된 된 세그먼트 인지 확인 **사용**, 다음을 사용 합니다.

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

및 **활성화/비활성화** 는 다음 사용 하 여 세그먼트를 지정 합니다.

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>분할된 된 컨트롤의 모양 수정

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
- Compact
- DefaultPrompt
- CompactPrompt

또한 사용 하 여 세그먼트 사이의 구분선을 설정할 수 있습니다.

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

여기서 첫 번째 `UIControlState` 구분선과 두 번째 부분의 세그먼트의 상태를 지정 `UIControlState` 오른쪽 세그먼트의 상태를 지정 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인과 Xamarin.tvOS 앱 내에서 분할 된 제어를 사용 하 여 작업 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
