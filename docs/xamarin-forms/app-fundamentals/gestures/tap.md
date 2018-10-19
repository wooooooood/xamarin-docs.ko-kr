---
title: 탭 제스처 인식기를 추가합니다.
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 탭 검색을 위한 탭 제스처를 사용 하는 방법을 설명 합니다. Tap 검색 TapGestureRecognizer 클래스를 사용 하 여 구현 됩니다.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: a28afb30770f15861aef06643e7f51070199ea9b
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/18/2018
ms.locfileid: "38994856"
---
# <a name="adding-a-tap-gesture-recognizer"></a>탭 제스처 인식기를 추가합니다.

_탭 제스처 탭 검색에 사용 되 고 TapGestureRecognizer 클래스를 사용 하 여 구현 됩니다._

탭 제스처를 사용 하 여 클릭할 수 있는 사용자 인터페이스 요소를 확인, 만들려면를 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 인스턴스를 처리 합니다 [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) 이벤트 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) 사용자 인터페이스 요소에는 컬렉션입니다. 다음 코드 예제는 `TapGestureRecognizer` 에 연결을 [ `Image` ](xref:Xamarin.Forms.Image) 요소:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

기본적으로 이미지는 단일 탭에 응답 합니다. 설정 된 [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) 속성을 두 번 눌러서 (또는 필요한 경우 더 많은 탭) 때까지 기다립니다.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

때 [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) 설정 된 하나 이상의 이벤트 처리기만 실행 됩니다 누르기가 (이 기간 아닙니다 구성 가능한) 설정된 기간 내에 발생 합니다. 두 번째 (또는 후속) 탭 해당 기간에 발생 하지 않은 경우는 실질적으로 무시 됩니다 및 '탭 수'를 다시 시작 합니다.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml을 사용 하 여

제스처 인식기는 연결 된 속성을 사용 하 여 Xaml에서 컨트롤에 추가할 수 있습니다. 추가할 구문을 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 이미지에는 다음과 같습니다 (이 경우 정의 *두 번 탭* 이벤트):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

(샘플)에서 이벤트 처리기의 코드를 카운터를 증가 및 색에서을 검정으로 이미지를 변경 &amp; 흰색입니다.

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

## <a name="using-icommand"></a>ICommand를 사용 하 여

일반적으로 모델-뷰-ViewModel (MVVM) 패턴을 사용 하는 응용 프로그램 사용 `ICommand` 이벤트 처리기를 직접 연결 하는 대신 합니다. [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) 쉽게 지원할 수 있습니다 `ICommand` 코드에서 바인딩을 설정 합니다.

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

또는 Xaml을 사용 합니다.

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

이 보기 모델에 대 한 전체 코드 샘플에 있습니다. 관련 `Command` 구현 세부 정보는 다음과 같습니다.

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

- [TapGesture (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
