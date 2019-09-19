---
title: XAML 미리 보기에서 사용자 지정 컨트롤 렌더링
description: 이 문서에서는 XAML 미리 보기에서 사용자 지정 컨트롤을 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 57c0fd540ef42c18462b4f989b21bac5ed05dc04
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105990"
---
# <a name="render-custom-controls-in-the-xaml-previewer"></a>XAML 미리 보기에서 사용자 지정 컨트롤 렌더링

_사용자 지정 컨트롤은 XAML 미리 보기에서 예상 대로 작동 하지 않을 수도 있습니다. 이 문서의 지침을 사용 하 여 사용자 지정 컨트롤 미리 보기의 제한 사항을 이해 합니다._

## <a name="basic-preview-mode"></a>기본 미리 보기 모드

프로젝트를 빌드하지 않았더라도 XAML 미리 보기에서 페이지가 렌더링 됩니다. 빌드 전 까지는 코드 숨김에 의존 하는 모든 컨트롤의 기본 Xamarin.ios 형식이 표시 됩니다. 프로젝트가 빌드되면 XAML 미리 보기에서 디자인 타임 렌더링을 사용 하는 사용자 지정 컨트롤을 표시 하려고 합니다. 렌더링에 실패 하면 기본 Xamarin.ios 형식이 표시 됩니다.

## <a name="enable-design-time-rendering-for-custom-controls"></a>사용자 지정 컨트롤에 디자인 타임 렌더링 사용

사용자 지정 컨트롤을 만들거나 타사 라이브러리의 컨트롤을 사용 하는 경우 미리 보기에서 해당 컨트롤을 잘못 표시 하는 경우도 있습니다. 사용자 지정 컨트롤은 디자인 타임 렌더링을 옵트인 (opt in) 하 여 컨트롤을 작성 하거나 라이브러리에서 가져왔는지 여부에 관계 없이 미리 보기에 표시 되도록 합니다. 사용자가 만든 컨트롤을 사용 하 여 [`[DesignTimeVisible(true)]`](xref:System.ComponentModel.DesignTimeVisibleAttribute) 를 컨트롤의 클래스에 추가 하 여 미리 보기에 표시 합니다.

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

[James Montemagno 'S ImageCirclePlugin's base 클래스](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs) 를 예로 사용 합니다.

## <a name="skiasharp-controls"></a>SkiaSharp 컨트롤

현재 SkiaSharp 컨트롤은 iOS에서 미리 보는 경우에만 지원 됩니다. Android 미리 보기에서는 렌더링 되지 않습니다.

## <a name="troubleshooting"></a>문제 해결

### <a name="check-your-xamarinforms-version"></a>Xamarin.ios 버전 확인
최소 Xamarin. 양식 3.6이 설치 되어 있는지 확인 합니다. NuGet에서 Xamarin.ios 버전을 업데이트할 수 있습니다.

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>를 사용 `[DesignTimeVisible(true)]`하는 경우에도 사용자 지정 컨트롤이 제대로 렌더링 되지 않습니다.
코드 숨김이 나 백 엔드 데이터를 많이 사용 하는 사용자 지정 컨트롤은 XAML 미리 보기에서 항상 작동 하지는 않습니다. 다음을 시도할 수 있습니다.

* [디자인 모드를 사용 하는](index.md#detect-design-mode) 경우 컨트롤을 초기화 하지 않도록 이동
* 백 엔드에서 가짜 데이터를 표시 하도록 [디자인 타임 데이터](design-time-data.md) 설정

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>XAML 미리 보기에는 "사용자 지정 컨트롤이 제대로 렌더링 되지 않습니다." 오류가 표시 됩니다.
프로젝트를 정리 하 고 다시 빌드하거나 XAML 파일을 닫았다가 다시 열어 보세요.
