---
title: Xamarin.ios MediaElement
description: 이 문서에서는 MediaElement를 사용 하 여 Xamarin.ios 응용 프로그램에서 비디오와 오디오를 재생 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/7/2020
ms.openlocfilehash: 67e4e9eec38f9dd23ebc3efe503d4b2a34ea8b39
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145720"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.ios MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)

`MediaElement` 비디오와 오디오를 재생 하기 위해 미디어 플레이어를 렌더링 합니다. 전송 컨트롤 이라고 하는 운영 체제 재생 컨트롤을 사용 하거나, 이러한 컨트롤을 숨기고 디자인 요구 사항에 맞게 사용자 고유의 컨트롤을 제공할 수 있습니다.

이 새로운 미리 보기 컨트롤을 사용 하려면 먼저 `App.xaml.xs`에서 적절 한 플래그를 설정 하 여 사용 하도록 옵트인 해야 합니다.

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

지원 되는 플랫폼:

- Android
- iOS
- UWP
- WPF
- macOS
- Tizen

## <a name="set-media-source"></a>미디어 원본 설정

`MediaElement` 속성 `Source`는 URI 또는 로컬 파일 경로를 사용할 수 있습니다. 미디어를 열 때 재생이 즉시 시작 됩니다.

```xaml
<MediaElement Source="http://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4" />
```

![](mediaelement-images/mediaelement-android.png "MediaElement on Android")

플랫폼 프로젝트에서 소스를 로컬 자산으로 설정 하려면 "ms appx" URI 체계를 사용 합니다.

```xaml
<MediaElement Source="ms-appx://XamarinShow_mid.mp4" />
```

데이터 바인딩을 사용 하는 경우 값 변환기를 사용 하 여이 URI 체계를 적용 하는 것이 좋습니다.

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (String.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xaml
<MediaElement
    Source="{Binding VideoSource, Converter={StaticResource VideoSourceConverter}}" />
```

## <a name="control-playback"></a>컨트롤 재생

기본적으로 `MediaElement`는 재생/일시 중지, 볼륨 및 음소거, 검색 및 시간 표시를 위한 플랫폼 네이티브 재생 컨트롤을 사용 합니다.

사용자 고유의 컨트롤을 재정의 하 고 구현 하려면 먼저 `ShowsPlaybackControls="False"`를 설정 하 여 플랫폼 컨트롤을 사용 하지 않도록 설정 합니다. 이제 다음 속성, 메서드 및 이벤트를 사용 하 여 재생 상태 및 컨트롤 재생을 반영 하기 위해 사용자 고유의 컨트롤을 사용할 수 있습니다.

- 재생을 제어 하는 `Play()`, `Pause()`, `Stop()` 메서드
- 초기 동작을 설정 `AutoPlay`
- 검색을 사용 하거나 사용 하지 않도록 `CanSeek`
- 시간에 대 한 `Position` 및 `Duration` 속성
- 볼륨 변경 및 음소거를 위한 `Volume` 속성
- 재생이 완료 되 면 반복 하기 위한 `IsLooping` 속성
- UI 업데이트 처리를 위한 `StateRequested`, `PositionRequested``VolumeRequested`

### <a name="handle-additional-media-events"></a>추가 미디어 이벤트 처리

`MediaOpened`, `MediaEnded`, `MediaFailed`및 `CurrentStateChanged` 이벤트와 같은 일반적인 미디어 이벤트에 응답할 수 있습니다. 항상 MediaFailed 이벤트를 처리 하는 것이 좋습니다.
CurrentStateChanged 재생 상태에 대 한 정보를 제공 합니다. `MediaElementState` 열거형의 값을 사용 합니다.

- **닫힘**: 미디어를 포함 하지 않으며 투명 프레임을 표시 합니다.
- **열기**: 유효성을 검사 하 고 지정 된 소스를 로드 하려고 합니다.
- **버퍼링**: 재생을 위해 미디어를 로드 하 고 있습니다.
  - 이 상태에서는 해당 `Position` 이동 하지 않습니다.
  - 비디오를 이미 재생 하는 경우 마지막으로 표시 된 프레임을 계속 표시 합니다.
- **재생**중: 현재 미디어 원본을 재생 하 고 있습니다.
- **일시 중지 됨**: `Position` 이동 하지 않습니다.
  - 비디오를 재생 하는 경우 계속 해 서 현재 프레임을 표시 합니다.
- **중지 됨**: 미디어를 포함 하지만 재생 중이거나 일시 중지 되지 않았습니다.
  - `Position`는 0 이며 이동 하지 않습니다.
  - 로드 된 미디어가 비디오 인 경우 `MediaElement` 첫 번째 프레임을 표시 합니다.

## <a name="control-display"></a>컨트롤 표시

[`Image`](xref:Xamarin.Forms.Image)와 마찬가지로 `MediaElement` 컨트롤에 `VideoHeight`, `VideoWidth`및 `Aspect`있습니다. 애스펙트는 다음과 같은 세 가지 옵션을 사용 합니다.

- 기능: 비디오에서 컨트롤의 전체 너비와 높이 **를 채우지만**, 종종 컨트롤 범위 밖으로 bleeding 원본 측면을 유지 합니다.
- 기능: 비디오는 원본 측면을 유지 하면서 컨트롤의 너비와 높이에 **맞게 조정 됩니다**.
- **Fill**: 비디오에서 컨트롤의 너비와 높이를 채웁니다.

## <a name="additional-properties"></a>추가 속성

- `VideoWidth`: 컨트롤의 너비입니다.
- `VideoHeight`: 컨트롤의 높이입니다.
- `KeepScreenOn`: 재생 하는 동안 장치 화면을 계속 유지 해야 하는 경우

## <a name="related-links"></a>관련 링크

- [MediaElement (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)
