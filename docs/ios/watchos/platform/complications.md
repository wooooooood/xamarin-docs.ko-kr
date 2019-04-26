---
title: watchOS Xamarin의 복잡성
description: 이 문서에서는 Xamarin에서 watchOS 복잡성을 사용 하는 방법을 설명 합니다. Complication, 템플릿을 작성 하는 complication 추가 하는 방법에 설명 하 고 샘플 코드를 제공 합니다.
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/03/2017
ms.openlocfilehash: 85b0c9b0688e9fb310a8f427018a02fe629404bb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61225546"
---
# <a name="watchos-complications-in-xamarin"></a>watchOS Xamarin의 복잡성

_watchOS는 조사식 얼굴에 대 한 사용자 지정 복잡성을 작성 하는 개발자를 수 있습니다._

이 페이지에서는 다양 한 유형의 사용 가능한 복잡성 및 watchOS 3 앱에는 복잡 한 상황을 추가 하는 방법을 설명 합니다.

참고 각 watchOS 응용 프로그램 하나 complication를 하나만 사용할 수 있습니다.

먼저 읽으십시오 [Apple 문서](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) 앱은 복잡 한 상황에 적합 한지 확인 하려면. 5 가지 `CLKComplicationFamily` 형식에서 선택할 수 표시:

[![](complications-images/all-complications-sml.png "5 CLKComplicationFamily 형식을 사용할 수 있습니다. 원형 소형 작은 모듈 형식 큰, Utilitarian 작은 모듈 형식, 큰 Utilitarian")](complications-images/all-complications.png#lightbox)

앱은 하나의 스타일 또는 5 개 모두에 표시 되는 데이터에 따라 구현할 수 있습니다.
또한 시간 이동, 사용자가 디지털 Crown 대로 지난 및/또는 이후 시간에 대 한 값을 제공을 지원할 수 있습니다.

<a name="adding" />

## <a name="adding-a-complication"></a>Complication 추가

### <a name="configuration"></a>구성

복잡성은 watch 앱을 만드는 동안 추가 또는 기존 솔루션에 수동으로 추가할 수 있습니다.

### <a name="add-new-project"></a>새 프로젝트 추가...

**새 프로젝트 추가...**  마법사에 자동으로 복잡성 컨트롤러 클래스를 만들고 구성 하는 확인란을 선택 합니다 **Info.plist** 파일:

![](complications-images/file-new-project-sml.png "Complication 포함 확인란")

### <a name="existing-projects"></a>기존 프로젝트

에 복잡 한 기존 프로젝트에 추가 합니다.

1. 새 **ComplicationController.cs** 클래스 파일 및 구현 `CLKComplicationDataSource`합니다.
2. 앱의 구성 **Info.plist** complication, 및 complication 패밀리는 지원 되는 id를 노출 합니다.

이러한 단계는 아래에서 자세히 설명 합니다.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource 클래스

다음 C# 구현에 필요한 최소 메서드를 포함 하는 서식 파일을 `CLKComplicationDataSource`합니다.

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

에 따라 합니다 [는 복잡 한 작성](#writing) 이 클래스에 코드를 추가 하는 지침입니다.

### <a name="infoplist"></a>Info.plist

조사식 확장 **Info.plist** 파일의 이름을 지정 해야 합니다 `CLKComplicationDataSource` 지원 하려는 complication 제품군 및:

[![](complications-images/complications-config-sml.png "Complication 제품군 형식")](complications-images/complications-config.png#lightbox)

합니다 **데이터 소스 클래스** 항목 목록 클래스 이름을 해당 서브 클래스에 표시 됩니다 `CLKComplicationDataSource` complication 논리를 포함 하는 하위 클래스입니다.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

모든 complication 기능이 단일 클래스에서 메서드 재정의에서 구현 되는 `CLKComplicationDataSource` 추상 클래스 (구현 하는 `ICLKComplicationDataSource` 인터페이스).

### <a name="required-methods"></a>필수 메서드

Complication 실행에 대 한 다음 메서드를 구현 해야 합니다.

- `GetPlaceholderTemplate` -앱을 제공할 수 없는 경우 또는 구성 하는 동안 사용 되는 정적 표시를 반환 합니다.
- `GetCurrentTimelineEntry` -Complication 실행 중일 때 올바른 표시를 계산 합니다.
- `GetSupportedTimeTravelDirections` --에서 옵션을 반환 하는 중 `CLKComplicationTimeTravelDirections` 와 같은 `None`, `Forward`합니다 `Backward`, 또는 `Forward | Backward`합니다.

### <a name="privacy"></a>개인 정보 보호

개인 데이터를 표시 하는 복잡성

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` 또는 `HideOnLockScreen`

이 메서드가 반환 하는 경우 `HideOnLockScreen` complication 중 하나 또는 응용 프로그램 이름 (아이콘과 아닌 데이터)에 표시 됩니다 시계 잠긴 경우.

### <a name="updates"></a>Updates

- `GetNextRequestedUpdateDate` -운영 체제 업데이트 complication 표시 데이터에 대 한 앱은 다음 쿼리 하는 경우 시간을 반환 합니다.

IOS 앱에서 업데이트를 강제로 수도 있습니다.

### <a name="supporting-time-travel"></a>지원 시간 이동

시간 여행 지원은 선택 사항에 의해 제어 되는 `GetSupportedTimeTravelDirections` 메서드. 반환 하는 경우 `Forward`하십시오 `Backward`, 또는 `Forward | Backward` 다음 메서드를 구현 해야 하는 다음

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>복잡 한 작성

단순 데이터에서 복잡성이 범위를 복잡 한 이미지 및 데이터 렌더링 시간 이동 지원과 표시 합니다. 아래 코드는 간단 하 고 단일 템플릿 complication 빌드하는 방법을 보여 줍니다.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>샘플 코드

이 예제에서는 지원 합니다 `UtilitarianLarge` 템플릿이 없으므로 complication의 해당 유형을 지 원하는 특정 조사식 면에만 선택할 수 있습니다. 때 *선택* 복잡성 표시를 watch **내 COMPLICATION** 시점과 *실행* 텍스트를 표시 **분 _시간_**   (부분과 시간).

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

각 complication 스타일에 대 한 사용 가능한 여러 템플릿의 여러 가지가 있습니다.
합니다 **링** 템플릿을 complication 진행률 또는 기타 값을 그래픽으로 표시 하려면 사용할 수 있는 주변 스타일 진행률 링을 표시할 수 있습니다.

[Apple의 CLKComplicationTemplate docs](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>원형 소형

이러한 템플릿 클래스 이름을 모두 사용 하 여 접두사가 `CLKComplicationTemplateCircularSmall`:

- **RingImage** -묶어 진행률 링을 사용 하 여 단일 이미지를 표시 합니다.
- **RingText** -묶어 진행률 링을 사용 하 여 텍스트 한 줄을 표시 합니다.
- **SimpleImage** -작은 단일 이미지를 표시 합니다.
- **SimpleText** -텍스트의 작은 코드 조각을 표시 합니다.
- **StackImage** -이미지 및 텍스트 상하로 줄 표시
- **StackText** -두 줄 텍스트를 표시 합니다.

### <a name="modular-small"></a>작은 모듈 형식

이러한 템플릿 클래스 이름을 모두 사용 하 여 접두사가 `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -(2 행과 2 열)의 텍스트 값의 작은 표를 표시 합니다.
- **RingImage** -묶어 진행률 링을 사용 하 여 단일 이미지를 표시 합니다.
- **RingText** -묶어 진행률 링을 사용 하 여 텍스트 한 줄을 표시 합니다.
- **SimpleImage** -작은 단일 이미지를 표시 합니다.
- **SimpleText** -텍스트의 작은 코드 조각을 표시 합니다.
- **StackImage** -이미지 및 텍스트 상하로 줄 표시
- **StackText** -두 줄 텍스트를 표시 합니다.

### <a name="modular-large"></a>큰 모듈 형식

이러한 템플릿 클래스 이름을 모두 사용 하 여 접두사가 `CLKComplicationTemplateModularLarge`:

- **열** -필요에 따라 각 행의 왼쪽에 이미지를 포함 하 여 2 개의 열, 3 행의 표를 표시 합니다.
- **StandardBody** -일반 텍스트의 두 행이 있는 굵은 헤더 문자열을 표시 합니다. 필요에 따라 헤더 왼쪽 이미지를 표시할 수 있습니다.
- **테이블** -2x2 표 아래에 있는 텍스트를 사용 하 여 굵은 헤더 문자열을 표시 합니다. 필요에 따라 헤더 왼쪽 이미지를 표시할 수 있습니다.
- **TallBody** -큰 글꼴 한 줄 텍스트 아래에 굵게 표시 된 헤더 문자열을 표시 합니다.

### <a name="utilitarian-small"></a>Utilitarian 작은

이러한 템플릿 클래스 이름을 모두 사용 하 여 접두사가 `CLKComplicationTemplateUtilitarianSmall`:

- **플랫** -(텍스트 짧은 이어야 함)는 한 줄에는 이미지 및 일부 텍스트를 표시 합니다.
- **RingImage** -묶어 진행률 링을 사용 하 여 단일 이미지를 표시 합니다.
- **RingText** -묶어 진행률 링을 사용 하 여 텍스트 한 줄을 표시 합니다.
- **사각형** -(40px 또는 44px는 38 mm 또는 42 mm Apple Watch 각각에 대 한 사각형) 정사각형 이미지를 표시 합니다.

### <a name="utilitarian-large"></a>큰 utilitarian

이 complication 스타일에 대 한 하나의 템플릿이: `CLKComplicationTemplateUtilitarianLargeFlat`합니다.
한 줄에 모두 단일 이미지와 일부 텍스트를 표시합니다.



## <a name="related-links"></a>관련 링크

- [Apple 문서](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC 비디오](https://developer.apple.com/videos/play/wwdc2015-209/)
