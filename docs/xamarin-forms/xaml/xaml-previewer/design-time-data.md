---
title: XAML 미리 보기에서 디자인 타임 데이터 사용
description: 이 문서에서는 디자인 타임 데이터를 사용 하 여 응용 프로그램을 실행 하지 않고 XAML 미리 보기에서 데이터를 많이 사용 하는 레이아웃을 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0F608019-5951-4BE6-80E0-9EEE1733D642
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: a6a34615adc9cf290ff6bf9dd344487e5f29cfa2
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887851"
---
# <a name="use-design-time-data-with-the-xaml-previewer"></a>XAML 미리 보기에서 디자인 타임 데이터 사용

_일부 레이아웃은 데이터 없이 시각화 하기가 어렵습니다. 이러한 팁을 사용 하 여 XAML 미리 보기에서 데이터를 많이 사용 하는 페이지의 미리 보기를 최대한 활용할 수 있습니다._

## <a name="design-time-data-basics"></a>디자인 타임 데이터 기본 사항

디자인 타임 데이터는 XAML 미리 보기에서 컨트롤을 더 쉽게 시각화할 수 있도록 설정 하는 가짜 데이터입니다. 시작 하려면 XAML 페이지의 헤더에 다음 코드 줄을 추가 합니다.

```xaml
xmlns:d="http://xamarin.com/schemas/2014/forms/design"
xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
mc:Ignorable="d"
```

네임 스페이스를 추가한 후에는 특성 `d:` 또는 컨트롤 앞에 배치 하 여 XAML 미리 보기에 표시할 수 있습니다. 가 있는 `d:` 요소는 런타임에 표시 되지 않습니다.

예를 들어 일반적으로 데이터가 바인딩된 레이블에 텍스트를 추가할 수 있습니다.

```xaml
<Label Text="{Binding Name}" d:Text="Name!" />
```

[![레이블에 텍스트를 사용 하 여 시간 데이터 디자인](xaml-previewer-images/designtimedata-label-sm.png "텍스트를 사용 하 여 시간 데이터 디자인")](xaml-previewer-images/designtimedata-label-lg.png#lightbox)

이 예제에서를 사용 `d:Text`하지 않으면 XAML 미리 보기에서 레이블에 아무것도 표시 되지 않습니다. 대신 "이름!"이 표시 됩니다. 여기서 레이블에는 런타임에 실제 데이터가 포함 됩니다.

색, 글꼴 `d:` 크기 및 간격과 같은 xamarin.ios 컨트롤의 특성과 함께를 사용할 수 있습니다. 컨트롤 자체에 추가할 수도 있습니다.

```xaml
<d:Button Text="Design Time Button" />
```

[![단추 컨트롤을 사용 하 여 시간 데이터 디자인](xaml-previewer-images/designtimedata-controls-sm.png "단추 컨트롤을 사용 하 여 시간 데이터 디자인")](xaml-previewer-images/designtimedata-controls-lg.png#lightbox)

이 예제에서 단추는 디자인 타임에만 표시 됩니다. 이 메서드를 사용 하 여 [XAML 미리 보기에서 지원 되지 않는 사용자 지정 컨트롤](render-custom-controls.md)에 대 한 자리 표시자를에 삽입할 수 있습니다.

## <a name="preview-images-at-design-time"></a>디자인 타임에 이미지 미리 보기

페이지에 바인딩되거나 동적으로 로드 되는 이미지에 대 한 디자인 타임 소스를 설정할 수 있습니다. Android 프로젝트에서 XAML 미리 보기에 표시 하려는 이미지를 **> 그릴** 수 있는 폴더에 리소스를 추가 합니다. IOS 프로젝트에서 **리소스** 폴더에 이미지를 추가 합니다. 그런 다음 디자인 타임에 XAML 미리 보기에 해당 이미지를 표시할 수 있습니다.

```xaml
<Image Source={Binding ProfilePicture} d:Source="DesignTimePicture.jpg" />
```

[![이미지를 사용 하 여 시간 데이터 디자인](xaml-previewer-images/designtimedata-image-sm.png "Iamges를 사용 하 여 디자인 타임 데이터")](xaml-previewer-images/designtimedata-image-lg.png#lightbox)

## <a name="design-time-data-for-listviews"></a>Listview에 대 한 디자인 타임 데이터

Listview은 모바일 앱에 데이터를 표시 하는 인기 있는 방법입니다. 그러나 실제 데이터 없이 시각화 하기가 어렵습니다. 디자인 타임 데이터를 사용 하려면 System.windows.controls.itemscontrol.itemssource으로 사용할 디자인 타임 배열을 만들어야 합니다. XAML 미리 보기는 디자인 타임에 ListView의 해당 배열에 있는 항목을 표시 합니다.

```xaml
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

[![ListView를 사용 하 여 디자인 타임 데이터](xaml-previewer-images/designtimedata-itemssource-sm.png "ListView를 사용 하 여 디자인 타임 데이터")](xaml-previewer-images/designtimedata-itemssource-lg.png#lightbox)

이 예제에서는 XAML 미리 보기에서 3 개의 TextCells의 ListView를 표시 합니다. 프로젝트의 기존 `x:String` 데이터 모델로 변경할 수 있습니다.

더 복잡 한 예제는 [James Montemagno 'S Hanselman 앱](https://github.com/jamesmontemagno/Hanselman.Forms/blob/vnext/src/Hanselman/Views/Podcasts/PodcastDetailsPage.xaml#L26-L47) 을 참조 하세요.

## <a name="alternative-hardcode-a-static-viewmodel"></a>대안과 정적 ViewModel 하드 코딩

디자인 타임 데이터를 개별 컨트롤에 추가 하지 않으려는 경우에는 해당 페이지에 바인딩할 모의 데이터 저장소를 설정할 수 있습니다. XAML에서 정적 ViewModel에 바인딩하는 방법을 보려면 James Montemagno의 [디자인 시 데이터 추가에 대한 블로그 게시물](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)을 참조하십시오.

## <a name="troubleshooting"></a>문제 해결

### <a name="requirements"></a>요구 사항

디자인 타임 데이터에는 최소 버전의 Xamarin. 양식 3.6이 필요 합니다.

### <a name="intellisense-shows-squiggly-lines-under-my-design-time-data"></a>IntelliSense가 디자인 타임 데이터 아래에 물결 모양의 선을 표시 합니다.

이것은 알려진 문제 이며 Visual Studio의 향후 버전에서 수정 될 예정입니다. 프로젝트는 여전히 오류 없이 빌드됩니다.

### <a name="the-xaml-previewer-stopped-working"></a>XAML 미리 보기의 작동이 중지 되었습니다.

XAML 파일을 닫았다가 다시 열어 프로젝트를 정리 하 고 다시 빌드 해 보세요.
