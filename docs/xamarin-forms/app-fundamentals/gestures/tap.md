---
title: 탭 제스처 인식기 추가
description: 이 문서에서는 Xamarin.Forms 애플리케이션에서 탭 감지를 위한 탭 제스처를 사용하는 방법을 설명합니다. 탭 감지는 TapGestureRecognizer 클래스를 사용하여 구현됩니다.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 95f25dbce55e2b960f604b6e304ffb6e8ed775e0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "70771333"
---
# <a name="adding-a-tap-gesture-recognizer"></a>탭 제스처 인식기 추가

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-tapgesture)

_탭 제스처는 탭 감지에 사용되며 TapGestureRecognizer 클래스를 통해 구현됩니다._

사용자 인터페이스 요소를 탭 제스처를 사용하여 클릭할 수 있도록 만들려면 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 인스턴스를 만들고, [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) 이벤트를 처리하고, 사용자 인터페이스 요소의 [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) 컬렉션에 새 제스처 인식기를 추가합니다. 다음 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 요소에 연결된 `TapGestureRecognizer`를 보여줍니다.

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

기본적으로 이미지는 단일 탭에 응답합니다. 두 번 탭(또는 필요한 경우 더 많은 탭)을 기다리도록 [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) 속성을 설정합니다.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

[`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired)가 1보다 크게 설정되면 설정된 일정 기간(이 기간은 구성할 수 없음) 내에 탭이 발생할 경우에만 이벤트 처리기가 실행됩니다. 두 번째(또는 후속) 탭이 해당 기간 내에 발생하지 않으면 실질적으로 무시되고 ‘탭 수’는 다시 시작됩니다.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>XAML 사용

제스처 인식기는 연결된 속성을 사용하여 XAML에서 컨트롤에 추가할 수 있습니다. 이미지에 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)를 추가할 구문은 다음과 같습니다(이 경우 *두 번 탭* 이벤트 정의).

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

샘플에서 이벤트 처리기의 코드는 카운터를 증가시키고 색의 이미지를 흑백으로 변경합니다.&amp;

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>ICommand 사용

MVVM(Model-View-ViewModel) 패턴을 사용하는 애플리케이션은 일반적으로 이벤트 처리기를 직접 연결하는 대신 `ICommand`를 사용합니다. [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)는 코드에서 바인딩을 설정하거나 XAML을 사용하여 손쉽게 `ICommand`를

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

지원할 수 있습니다.

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

이 보기 모델에 대한 전체 코드는 샘플에서 확인할 수 있습니다. 관련 `Command` 구현 세부 정보는 다음과 같습니다.

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## <a name="related-links"></a>관련 링크

- [TapGesture(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-tapgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
