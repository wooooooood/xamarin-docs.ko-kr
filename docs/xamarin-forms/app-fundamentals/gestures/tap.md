---
title: Tap 제스처 제스처 인식기를 추가합니다.
description: 이 문서에서는 Xamarin.Forms 응용 프로그램에서 tap 검색을 위한 탭 제스처를 사용 하는 방법을 설명 합니다. Tap 검색 TapGestureRecognizer 클래스로 구현 됩니다.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: bbe4ca7a1080459b8aeb33640be5158b15e97715
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240667"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>Tap 제스처 제스처 인식기를 추가합니다.

_탭 제스처 tap 검색에 사용 되 고 TapGestureRecognizer 클래스를 사용 하 여 구현 됩니다._

## <a name="overview"></a>개요

사용자 인터페이스 요소를 탭 제스처를 클릭할 수 있도록 만들는 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 인스턴스를 처리는 [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) 이벤트를 새 제스처 인식기를 추가 하 고는 [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) 사용자 인터페이스 요소의 컬렉션입니다. 다음 코드 예제는 `TapGestureRecognizer` 에 연결 된 프로그램 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

기본적으로 이미지는 단일 탭에 응답 합니다. 설정의 [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) 속성을 두 번 누르면 (또는 필요한 경우 더 많은 탭) 때까지 대기 합니다.

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

때 [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/) 설정, 하나 이상의 이벤트 처리기만 실행 됩니다 누르기가 설정된 일정 기간 (기간 이것이 구성 가능) 내에서 발생 하는 경우. 두 번째 (또는 후속) 탭 기간 내에 발생 하지 않습니다는 효과적으로 무시 되 고 'tap count' 다시 시작 합니다.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml을 사용 하 여

제스처 인식기에서 연결 된 속성을 사용 하 여 Xaml 컨트롤에 추가할 수 있습니다. 추가 하는 구문은 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 이미지에는 다음과 같습니다 (이 경우 정의 *두 번 탭* 이벤트):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

(샘플)의 이벤트 처리기에 대 한 코드 카운터를 증가 시키고 이미지에서 색을 검정으로 변경 &amp; 흰색입니다.

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

일반적으로 Mvvm 패턴을 사용 하는 응용 프로그램 사용 `ICommand` 직접 배선 이벤트 처리기를 대신 합니다. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 쉽게 지원할 수 `ICommand` 바인딩을 코드에 설정 중 하나:

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

이 보기 모델에 대 한 전체 코드 샘플에 있습니다. 관련 `Command` 구현 세부 내용은 다음과 같습니다.

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

## <a name="summary"></a>요약

탭 제스처 tap 검색에 사용 되 고 사용 하 여 구현 된 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) 클래스입니다. 탭의 수를 두 번 누르면 인식 하도록 지정할 수 있습니다 (또는 세 번 누르기 이상의 탭) 동작 합니다.


## <a name="related-links"></a>관련 링크

- [TapGesture (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
