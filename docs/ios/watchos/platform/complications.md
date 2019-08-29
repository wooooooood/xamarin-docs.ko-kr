---
title: Xamarin에서 watchOS의 복잡 한 문제
description: 이 문서에서는 Xamarin에서 watchOS 복잡 한 작업을 수행 하는 방법을 설명 합니다. 이 문서에서는 복잡 한 기능을 추가 하 고, 복잡 한 템플릿을 작성 하 고, 샘플 코드를 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/03/2017
ms.openlocfilehash: 3fc40f3a422901d0ffb4779e0c0b7c46de052d3a
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121206"
---
# <a name="watchos-complications-in-xamarin"></a>Xamarin에서 watchOS의 복잡 한 문제

_watchOS를 사용 하면 개발자가 시청 얼굴에 대 한 사용자 지정 문제를 작성할 수 있습니다._

이 페이지에서는 사용 가능한 다양 한 종류의 복잡 한 유형과 watchOS 3 앱에 복잡 한를 추가 하는 방법을 설명 합니다.

각 watchOS 응용 프로그램은 한 가지 복잡 한 경우에만 사용할 수 있습니다.

먼저 [Apple의 문서](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) 를 읽고 앱이 복잡 한 경우에 적합 한지 확인 합니다. 선택할 수 있는 `CLKComplicationFamily` 표시 유형에는 다음 5 가지가 있습니다.

[![](complications-images/all-complications-sml.png "사용할 수 있는 5 가지 CLKComplicationFamily 형식은 다음과 같습니다. 원형 Small, 모듈형 Small, 모듈식 큼, 실속형 Small, 실속형 Large")](complications-images/all-complications.png#lightbox)

앱은 표시 되는 데이터에 따라 스타일을 하나 또는 5 개만 구현할 수 있습니다.
사용자가 Digital Crown를 전환할 때 과거 및/또는 미래의 시간에 대 한 값을 제공 하 여 시간 이동을 지원할 수도 있습니다.

<a name="adding" />

## <a name="adding-a-complication"></a>복잡 한 추가

### <a name="configuration"></a>Configuration

응용 프로그램을 만드는 동안 조사식 응용 프로그램에 추가 하거나 기존 솔루션에 수동으로 추가할 수 있습니다.

### <a name="add-new-project"></a>새 프로젝트 추가 ...

**새 프로젝트 추가 ...** 마법사에는 복잡 한 컨트롤러 클래스를 자동으로 만들고 **info.plist** 파일을 구성 하는 확인란이 포함 됩니다.

![](complications-images/file-new-project-sml.png "복잡 한 내용 포함 확인란")

### <a name="existing-projects"></a>기존 프로젝트

기존 프로젝트에 복잡 한 프로젝트를 추가 하려면 다음을 수행 합니다.

1. 새 **ComplicationController.cs** 클래스 파일을 만들고을 구현 `CLKComplicationDataSource`합니다.
2. 응용 프로그램의 **info.plist** 를 구성 하 여 복잡 한 패밀리를 노출 하 고 지원 되는 복잡 한 패밀리를 식별 합니다.

이러한 단계는 아래에 자세히 설명 되어 있습니다.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource 클래스

다음 C# 템플릿에서는을 `CLKComplicationDataSource`구현 하는 데 필요한 최소 메서드를 제공 합니다.

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

[복잡 한 지침 작성](#writing) 을 수행 하 여이 클래스에 코드를 추가 합니다.

### <a name="infoplist"></a>Info.plist

Watch 확장의 **info.plist** 파일은 지원 하려는 `CLKComplicationDataSource` 및 복잡 한 제품군의 이름을 지정 해야 합니다.

[![](complications-images/complications-config-sml.png "복잡 한 패밀리 형식")](complications-images/complications-config.png#lightbox)

**데이터 소스 클래스** 항목 목록에는 복잡 한 논리를 포함 `CLKComplicationDataSource` 하는 하위 클래스의 클래스 이름이 표시 됩니다.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

모든 복잡 한 기능은 단일 클래스에서 구현 되며, `CLKComplicationDataSource` `ICLKComplicationDataSource` 인터페이스를 구현 하는 추상 클래스의 메서드를 재정의 합니다.

### <a name="required-methods"></a>필수 메서드

복잡 한 실행을 위해 다음 메서드를 구현 해야 합니다.

- `GetPlaceholderTemplate`-구성 중 또는 앱에서 값을 제공할 수 없는 경우에 사용 되는 정적 표시를 반환 합니다.
- `GetCurrentTimelineEntry`-복잡 한를 실행 하는 경우 올바른 디스플레이를 계산 합니다.
- `GetSupportedTimeTravelDirections`- `None`,, 또는 `CLKComplicationTimeTravelDirections` `Forward` `Backward`등의 옵션을 반환 합니다. `Forward | Backward`

### <a name="privacy"></a>개인 정보 취급 방침

개인 데이터를 표시 하는 복잡 한

- `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` 또는 `HideOnLockScreen`

이 메서드가를 반환 `HideOnLockScreen` 하는 경우 조사식이 잠기면 아이콘이 나 응용 프로그램 이름 (데이터 아님)이 표시 됩니다.

### <a name="updates"></a>업데이트

- `GetNextRequestedUpdateDate`-운영 체제가 다음에 앱을 쿼리하여 업데이트 된 복잡 한 표시 데이터를 반환 해야 하는 시간을 반환 합니다.

IOS 앱에서 강제로 업데이트할 수도 있습니다.

### <a name="supporting-time-travel"></a>지원 시간 이동

시간 이동 지원은 선택 사항이 며 `GetSupportedTimeTravelDirections` 메서드에 의해 제어 됩니다. , `Forward` `Backward`또는 를반환하는경우다음메서드를구현해야합니다.`Forward | Backward`

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>복잡 한 작성

간단한 데이터 표시부터 시간 이동 지원과 관련 된 복잡 한 이미지 및 데이터 렌더링에 이르기까지 다양 합니다. 아래 코드에서는 간단한 단일 템플릿을 복잡 하 게 만드는 방법을 보여 줍니다.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>예제 코드

이 예제에서는 `UtilitarianLarge` 템플릿만 지원 하므로 해당 유형을 지 원하는 특정 조사식 면 에서만 선택할 수 있습니다. 조사식을 *선택* 하는 경우 복잡 한 것 을 표시 하 고 *실행* 하면 텍스트 분 ( **시간**  부분 포함)을 표시 합니다.

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

## <a name="complication-templates"></a>복잡 한 템플릿

각 복잡 한 스타일에 사용할 수 있는 다양 한 템플릿이 있습니다.
**링** 템플릿을 사용 하면 진행률 또는 기타 일부 값을 그래픽으로 표시 하는 데 사용 될 수 있는 복잡 한 진행률 스타일 링을 표시할 수 있습니다.

[Apple의 CLKComplicationTemplate 문서](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>원형 작은

이러한 템플릿 클래스 이름 앞에 `CLKComplicationTemplateCircularSmall`는 모두 접두사가 붙습니다.

- **RingImage** -단일 이미지를 표시 하 고 그 주위에 진행률 링을 표시 합니다.
- **RingText** -한 줄의 텍스트를 표시 하 고 진행률 고리를 표시 합니다.
- **SimpleImage** -작은 단일 이미지를 표시 하기만 하면 됩니다.
- **SimpleText** -작은 텍스트 조각을 표시 하기만 하면 됩니다.
- **Stackimage** -이미지와 텍스트 한 줄을 표시 합니다.
- **Stacktext** -두 줄의 텍스트를 표시 합니다.

### <a name="modular-small"></a>모듈식 소형

이러한 템플릿 클래스 이름 앞에 `CLKComplicationTemplateModularSmall`는 모두 접두사가 붙습니다.

- **ColumnsText** -작은 텍스트 값 표 (행 2 개 및 열 2 개)를 표시 합니다.
- **RingImage** -단일 이미지를 표시 하 고 그 주위에 진행률 링을 표시 합니다.
- **RingText** -한 줄의 텍스트를 표시 하 고 진행률 고리를 표시 합니다.
- **SimpleImage** -작은 단일 이미지를 표시 하기만 하면 됩니다.
- **SimpleText** -작은 텍스트 조각을 표시 하기만 하면 됩니다.
- **Stackimage** -이미지와 텍스트 한 줄을 표시 합니다.
- **Stacktext** -두 줄의 텍스트를 표시 합니다.

### <a name="modular-large"></a>모듈식 큼

이러한 템플릿 클래스 이름 앞에 `CLKComplicationTemplateModularLarge`는 모두 접두사가 붙습니다.

- **열** -두 개의 열이 있는 3 개 행의 표를 표시 합니다. 각 행의 왼쪽에 이미지를 포함 시킬 수도 있습니다.
- **Standardbody** -일반 텍스트 행 두 개가 있는 굵은 머리글 문자열을 표시 합니다. 머리글은 선택적으로 왼쪽에 이미지를 표시할 수 있습니다.
- **테이블** -굵은 머리글 문자열을 표시 하 고 그 아래에 2x2 표를 표시 합니다. 머리글은 선택적으로 왼쪽에 이미지를 표시할 수 있습니다.
- **TallBody** -아래에 더 큰 글꼴 텍스트 한 줄이 있는 굵은 머리글 문자열을 표시 합니다.

### <a name="utilitarian-small"></a>실속형 Small

이러한 템플릿 클래스 이름 앞에 `CLKComplicationTemplateUtilitarianSmall`는 모두 접두사가 붙습니다.

- **Flat** -이미지와 일부 텍스트를 한 줄에 표시 합니다. 텍스트는 짧게 표시 됩니다.
- **RingImage** -단일 이미지를 표시 하 고 그 주위에 진행률 링을 표시 합니다.
- **RingText** -한 줄의 텍스트를 표시 하 고 진행률 고리를 표시 합니다.
- **사각형** -사각형 이미지를 표시 합니다. 즉, Apple Watch 38mm에 대 한 사각형 이미지 (40px 또는 44px square)를 각각 표시 합니다.

### <a name="utilitarian-large"></a>실속형 큼

이 복잡 한 스타일 `CLKComplicationTemplateUtilitarianLargeFlat`에는 템플릿이 하나만 있습니다.
단일 이미지와 일부 텍스트를 한 줄에 표시 합니다.



## <a name="related-links"></a>관련 링크

- [Apple의 문서](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC 비디오](https://developer.apple.com/videos/play/wwdc2015-209/)
