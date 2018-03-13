---
title: "복잡성"
description: "watchOS는 조사식에 대해 사용자 지정 복잡성을 작성 하는 개발자를 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: affe58d9276bd0b687089fb42a14ca964c570c9c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="complications"></a>복잡성

_watchOS는 조사식에 대해 사용자 지정 복잡성을 작성 하는 개발자를 수 있습니다._

이 페이지에서는 다양 한 유형의 사용할 수 있는 복잡성 및 watchOS 3 응용 프로그램에 복잡 함 중 하나를 추가 하는 방법을 설명 합니다.

참고 각 watchOS 응용 프로그램 하나 복잡 함 중 하나를 하나만 사용할 수 있습니다.

먼저 읽으십시오 [Apple의 docs](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) 는 복잡 한 상황에 적합 한 응용 프로그램 인지 여부를 확인 합니다. 5 가지 `CLKComplicationFamily` 유형의 디스플레이에서 선택할 수 있습니다.

[![](complications-images/all-complications-sml.png "지원 되는 5 CLKComplicationFamily 유형을: 순환 작은, 모듈식 작은, 모듈식 큰, 상태로 이해 하기 쉽게 작은, 상태로 이해 하기 쉽게 큰")](complications-images/all-complications.png#lightbox)

응용 프로그램은 하나의 스타일 또는 모든 다섯 개, 표시 되는 데이터에 따라 구현할 수 있습니다.
사용자가 디지털 왕관을 지난 및/또는 이후 시간에 대 한 값을 제공 하는 시간 이동을 지원할 수 있습니다.

<a name="adding" />

## <a name="adding-a-complication"></a>복잡 함 중 하나를 추가합니다.

### <a name="configuration"></a>구성

복잡성을 만드는 동안 watch 앱에 추가 또는 기존 솔루션에 수동으로 추가할 수 있습니다.

### <a name="add-new-project"></a>새 프로젝트 추가...

**새 프로젝트 추가...**  마법사에 자동으로 complication 컨트롤러 클래스를 만들고 구성 하는 확인란은 **Info.plist** 파일:

![](complications-images/file-new-project-sml.png "Complication 포함 확인란")

### <a name="existing-projects"></a>기존 프로젝트

에 복잡 한 기존 프로젝트에 추가 합니다.

1. 새 **ComplicationController.cs** 클래스 파일과 구현 `CLKComplicationDataSource`합니다.
2. 응용 프로그램의 구성 **Info.plist** 는 복잡 함 중 하나를 및 사용할 수 있는 complication 패밀리 id를 노출 하도록 합니다.

이러한 단계는 아래에서 자세히 설명 되어 있습니다.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource 클래스

다음 C# 서식 파일을 구현 하는 최소 필수 메서드를 포함 한 `CLKComplicationDataSource`합니다.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

에 따라는 [는 복잡 한 작성](#writing) 이 클래스에 코드를 추가 하는 지침입니다.

### <a name="infoplist"></a>Info.plist

조사식 확장 **Info.plist** 파일의 이름을 지정 해야는 `CLKComplicationDataSource` 어떤 complication 패밀리를 지원 하려면 및:

[![](complications-images/complications-config-sml.png "Complication 패밀리 형식")](complications-images/complications-config.png#lightbox)

**데이터 소스 클래스** 항목 목록 클래스 이름을 해당 하위 클래스에 표시 됩니다 `CLKComplicationDataSource` complication 논리를 포함 하는 하위 클래스입니다.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

모든 complication 기능에서 메서드를 재정의 하는 단일 클래스에서 구현 되는 `CLKComplicationDataSource` 추상 클래스 (구현 하는 `ICLKComplicationDataSource` 인터페이스).

### <a name="required-methods"></a>필수 메서드

실행 하는 복잡 함 중 하나에 대 한 다음 메서드를 구현 해야 합니다.

- `GetPlaceholderTemplate` -응용 프로그램 값을 제공할 수 없는 경우 나 구성 중에 사용 정적 표시를 반환 합니다.
- `GetCurrentTimelineEntry` -Complication 실행 중일 때 올바른 표시를 계산 합니다.
- `GetSupportedTimeTravelDirections` --에서 옵션을 반환 하는 중 `CLKComplicationTimeTravelDirections` 같은 `None`, `Forward`, `Backward`, 또는 `Forward | Backward`합니다.

### <a name="privacy"></a>개인 정보 보호

개인 데이터를 표시 하는 복잡성

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` 또는 `HideOnLockScreen`

이 메서드가 반환 하는 경우 `HideOnLockScreen` complication 중 하나 또는 응용 프로그램 이름 (아이콘과 지방의) 표시 됩니다 시계 잠겨 있을 때.

### <a name="updates"></a>Updates

- `GetNextRequestedUpdateDate` -운영 체제 응용 프로그램 데이터를 표시 하는 업데이트 된 복잡 함 중 하나에 대 한 다음 쿼리하여 때 한 번을 반환 합니다.

또한 iOS 앱에서 업데이트를 적용할 수 있습니다.

### <a name="supporting-time-travel"></a>시간 이동 지원

시간 여행 지원은 며에 의해 제어 되는 `GetSupportedTimeTravelDirections` 메서드. 반환 하는 경우 `Forward`, `Backward`, 또는 `Forward | Backward` 다음 메서드를 구현 해야 합니다

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>복잡 한 작성

복잡성 범위는 간단한 데이터에서 복잡 한 이미지와 시간 이동 지원 데이터 렌더링에 표시 합니다. 아래 코드는 간단 하 고 단일 템플릿 복잡 함 중 하나를 빌드하는 방법을 보여 줍니다.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>샘플 코드

이 예에서는 지원는 `UtilitarianLarge` 템플릿이 complication의 해당 형식을 지 원하는 특정 조사식 면에만 선택할 수 있습니다. 때 *선택* 복잡 한 조사식에서 표시 **내 COMPLICATION** 및 시기 *실행* 텍스트를 표시 **분 _시간_**   (부분과 시간).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Complication 템플릿

각 complication 스타일에 대해 사용 가능한 여러 템플릿의 수가 있습니다.
**링** 템플릿을 사용 하면 진행률 또는 일부 다른 값을 그래픽으로 표시 하는 데 사용할 수 있는 complication 주변 스타일 진행률 링을 표시 합니다.

[Apple의 CLKComplicationTemplate docs](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>순환 작은

이러한 템플릿 클래스 이름은 모두와 접두사가 `CLKComplicationTemplateCircularSmall`:

- **RingImage** -주위 진행률 링으로 단일 이미지를 표시 합니다.
- **RingText** -진행률 링 주위에 있는 텍스트 한 줄을 표시 합니다.
- **SimpleImage** -작은 단일 이미지를 표시 합니다.
- **SimpleText** -텍스트의 작은 조각 표시 합니다.
- **StackImage** -이미지와 상하로 텍스트 줄을 표시 합니다.
- **StackText** -두 줄 텍스트를 표시 합니다.

### <a name="modular-small"></a>모듈식 작은

이러한 템플릿 클래스 이름은 모두와 접두사가 `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -(2 개 행 및 열을 2) 텍스트 값의 작은 표를 표시 합니다.
- **RingImage** -주위 진행률 링으로 단일 이미지를 표시 합니다.
- **RingText** -진행률 링 주위에 있는 텍스트 한 줄을 표시 합니다.
- **SimpleImage** -작은 단일 이미지를 표시 합니다.
- **SimpleText** -텍스트의 작은 조각 표시 합니다.
- **StackImage** -이미지와 상하로 텍스트 줄을 표시 합니다.
- **StackText** -두 줄 텍스트를 표시 합니다.

### <a name="modular-large"></a>큰 모듈

이러한 템플릿 클래스 이름은 모두와 접두사가 `CLKComplicationTemplateModularLarge`:

- **열** -필요에 따라 각 행의 왼쪽에 이미지를 포함 한 2 개의 열, 3 행의 표를 표시 합니다.
- **StandardBody** -일반 텍스트의 두 행이 있는 굵게 표시 된 헤더 문자열을 표시 합니다. 필요에 따라 헤더 왼쪽에는 이미지를 표시할 수 있습니다.
- **테이블** -그 아래에 있는 텍스트의 2x2 그리드와 굵게 표시 된 헤더 문자열을 표시 합니다. 필요에 따라 헤더 왼쪽에는 이미지를 표시할 수 있습니다.
- **TallBody** -큰 글꼴 한 줄의 텍스트 아래에 굵게 표시 된 헤더 문자열을 표시 합니다.

### <a name="utilitarian-small"></a>상태로 이해 하기 쉽게 작은

이러한 템플릿 클래스 이름은 모두와 접두사가 `CLKComplicationTemplateUtilitarianSmall`:

- **플랫** -(텍스트 짧은 이어야 함)을 한 줄에는 이미지 및 일부 텍스트를 표시 합니다.
- **RingImage** -주위 진행률 링으로 단일 이미지를 표시 합니다.
- **RingText** -진행률 링 주위에 있는 텍스트 한 줄을 표시 합니다.
- **정사각형** -(40px 또는 44px는 38 m m 또는 42 mm Apple Watch 각각에 대 한 사각형) 정사각형 이미지를 표시 합니다.

### <a name="utilitarian-large"></a>상태로 이해 하기 쉽게 큰

이 complication 스타일에 대 한 하나의 템플릿이: `CLKComplicationTemplateUtilitarianLargeFlat`합니다.
단일 이미지와 텍스트를 모두 한 줄에 표시합니다.



## <a name="related-links"></a>관련 링크

- [Apple의 docs](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC 비디오](https://developer.apple.com/videos/play/wwdc2015-209/)
