---
title: XAML 미리 보기를 사용 하 여 디자인 타임 데이터를 사용 하 여
description: 이 문서에서는 앱을 실행 하지 않고 XAML 미리 보기에서 데이터 중심 레이아웃을 표시 하려면 디자인 타임 데이터를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 0ff9f8b5ee6f9468650b6535745706bee8f96536
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60876357"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>XAML 미리 보기를 사용 하 여 디자인 타임 데이터를 사용 하 여

_일부 레이아웃은 데이터가 없는 시각화 하기가 어렵습니다. XAML 미리 보기에서 데이터 양이 많은 페이지를 미리 보기를 최대한 활용 되도록 다음이 팁을 사용 합니다._

## <a name="design-time-data-basics"></a>디자인 타임 데이터 기본 사항

디자인 타임 데이터는 컨트롤을 XAML 미리 보기에서 시각화를 쉽게 수행할 수 있도록 설정 하는 모조 데이터입니다. 시작 하려면 XAML 페이지의 헤더에 코드의 다음 줄을 추가 합니다.

```csharp
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

네임 스페이스를 추가한 후 넣으면 `d:` 모든 특성 또는 XAML 미리 보기에서 표시할 컨트롤 앞에 있습니다. 요소 `d:` 런타임에 표시 되지 않습니다.

예를 들어, 일반적으로 데이터를 바인딩할 수 있는 레이블에 텍스트를 추가할 수 있습니다.

```csharp
<Label Text={Binding Name} d:Text="Name" />
```

[![디자인 타임 데이터 레이블의 텍스트를 사용 하 여](xaml-previewer-images/designtimedata-label-sm.png "디자인 시간 텍스트를 사용 하 여 데이터 레이블")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

 이 예제에서는 없이 `d:Text`, XAML 미리 보기는 레이블에 대 한 아무 것도 표시 됩니다. 대신, 표시 "Name" 레이블을 런타임에 실제 데이터를가 됩니다.

사용할 수 있습니다 `d:` 색, 글꼴 크기를 등 간격 Xamarin.Forms 컨트롤에 대 한 모든 특성을 사용 하 여 합니다. 컨트롤 자체에 추가할 수 있습니다.

```csharp
<d:Button Text="Design Time Button" />
```

[![단추 컨트롤을 사용 하 여 시간 데이터를 디자인할](xaml-previewer-images/designtimedata-controls-sm.png "단추 컨트롤을 사용 하 여 시간 데이터를 디자인 합니다.")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

이 예제에서는 단추 디자인 타임에만 표시 됩니다. 이 메서드를 사용 하 여 자리 표시자에 배치 된 [XAML 미리 보기에서 지원 되지 않습니다 사용자 지정 컨트롤](render-custom-controls.md)합니다.

## <a name="preview-images-at-design-time"></a>디자인 타임 미리 보기 이미지

페이지에 연결 되거나 동적으로 로드 되는 이미지에 대 한 디자인 타임 소스를 설정할 수 있습니다. Android 프로젝트에서 XAML 미리 보기에 표시 하려는 이미지를 추가 합니다 **리소스 > Drawable** 폴더입니다. IOS 프로젝트에서 이미지를 추가 합니다 **리소스** 폴더입니다. 그런 다음 디자인 타임에 XAML 미리 보기에서 해당 이미지를 표시할 수 있습니다.

```csharp
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```
[![이미지를 사용 하 여 시간 데이터 디자인](xaml-previewer-images/designtimedata-image-sm.png "iamges 사용 하 여 시간 데이터를 디자인 합니다.")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Listview에 대 한 디자인 타임 데이터

Listview는 모바일 앱에서 데이터를 표시 하는 인기 있는 방법입니다. 그러나 실제 데이터가 없으므로 시각화 하기가 어렵습니다. 사용 하 여 디자인 타임 데이터를 사용 하려면 ItemsSource로 사용할 디자인 시간 배열을 만들려면 해야 합니다. XAML 미리 보기는 디자인 타임에 ListView에서 해당 배열에 포함 된 내용 표시 합니다.

```csharp
<StackLayout>
    <ListView ItemsSource="{Binding Items}">
        <d:ListView.ItemsSource>
            <x:Array Type="{x:Type x:String}">
                <x:String>Item One</x:String>
                <x:String>Item Two</x:String>
                <x:String>Item Three</x:String>
            </x:Array>
        </d:ListView.ItemsSource>
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding ItemName}"
                          d:Text="{Binding .}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackLayout>
```

[![디자인 타임 데이터를 ListView 사용 하 여](xaml-previewer-images/designtimedata-itemssource-sm.png "디자인 타임 데이터를 ListView 사용 하 여")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

이 예제는 세 가지 TextCells의 ListView XAML 미리 보기에서 표시 됩니다. 변경할 수 있습니다 `x:String` 프로젝트에서 기존 데이터 모델에 있습니다.

가리킵니다 [James Montemagno의 Hanselman.Forms 앱](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L36-L57) 더 복잡 한 예입니다.


## <a name="alternative-hardcode-a-static-viewmodel"></a>대체: 하드 코드 된 정적 ViewModel

개별 컨트롤에 디자인 타임 데이터를 추가할 않으려면 페이지로 바인딩할 모의 데이터 저장소를 설정할 수 있습니다. XAML에서 정적 ViewModel에 바인딩하는 방법을 보려면 James Montemagno의 [디자인 시 데이터 추가에 대한 블로그 게시물](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)을 참조하십시오.

## <a name="troubleshooting"></a>문제 해결

### <a name="requirements"></a>요구 사항

디자인 타임 데이터에는 Xamarin.Forms 3.6 이상이 필요합니다.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense에는 디자인 타임 데이터 아래에 구불구불한 선을 표시 됩니다.

이 알려진된 문제 및 Visual Studio의 향후 버전에서 수정 될 예정입니다. 프로젝트가 오류 없이 계속 빌드됩니다.

### <a name="the-xaml-previewer-stopped-working"></a>XAML 미리 보기 작동이 중지 되었습니다.

여세요 및 XAML 파일을 열어 정리 및 프로젝트를 다시 작성 합니다.
