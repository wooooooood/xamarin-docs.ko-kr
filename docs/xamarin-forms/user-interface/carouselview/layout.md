---
title: Xamarin.ios CarouselView
description: 기본적으로 CarouselView에는 해당 항목이 가로 목록으로 표시 됩니다. 그러나 세로 방향을 포함 하 여 CollectionView와 동일한 레이아웃에도 액세스할 수 있습니다.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: pauldipietro
ms.author: padipi
ms.date: 09/09/2019
ms.openlocfilehash: 5bbaeead524089ea604cfd9a9fd7f3a85d04e724
ms.sourcegitcommit: e83035c746f165ee6d03f2e9fd0066ee4f20a9fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70908345"
---
# <a name="xamarinforms-carouselview-layouts"></a>Xamarin.ios CarouselView 레이아웃

![](~/media/shared/preview.png "이 API는 현재 시험판임")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)

## <a name="introduction"></a>소개

CarouselView에서 사용할 수 있는 대부분의 레이아웃 기능은 CollectionView에서 발생 합니다. CollectionView의 [레이아웃 설명서](../collectionview/layout.md) 를 참조 하 여 다양 한 레이아웃을 사용 하는 방법을 확인할 수 있습니다.

## <a name="differences-from-collectionview"></a>CollectionView의 차이점

기본적으로 CarouselView 항목은 응용 프로그램에서의 일반적인 기능 처럼 가로 방향으로 되어 있습니다.

또한 CarouselView는 다음과 같은 몇 가지 추가 속성을 제공 합니다.

> [!IMPORTANT]
> CarouselView에 대 한 추가 속성이 아직 개발 중 이며이 목록은 아직 완료 되지 않았습니다.

| API | 함수 |
|---|---|---|
| Numberof함께 항목 | 현재 항목의 각 면에 표시 되는 항목의 수를 설정 합니다. 기본값은 0입니다.
| PeekAreaInsets | 현재 항목 옆에 표시 되는 표시 수준을 조정 하 여 CarouselView에 스크롤할 추가 항목이 있음을 사용자에 게 시각적으로 표시 하는 방법을 제공 합니다.

## <a name="setting-the-number-of-fully-visible-items"></a>완전히 표시 되는 항목 수 설정

기본적으로 CarouselView는 전체 화면에서 한 항목을 표시 합니다. 사용자는 `NumberOfSideItems` 속성을 설정 하 여 더 많은 항목이 현재 항목 옆에 표시 되도록 할 수 있습니다. 로 `PeekAreaInsets` 설정 된 값은 여전히 적용 됩니다.

```xaml
<StackLayout Margin="20">
    <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" NumberOfSideItems="1">
        <CarouselView.ItemTemplate>
            <DataTemplate>
                <Frame BorderColor="Black">
                    <Grid>
                        <Grid.RowDefinitions>
                            <RowDefinition Height="Auto" />
                            <RowDefinition Height="Auto" />
                        </Grid.RowDefinitions>
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="Auto" />
                            <ColumnDefinition Width="Auto" />
                        </Grid.ColumnDefinitions>
                        <Label Grid.Column="1"
                            Text="{Binding Name}"
                            FontAttributes="Bold"
                            FontSize="14"/>
                        <Label Grid.Row="1"
                            Grid.Column="1"
                            Text="{Binding Location}"
                            FontAttributes="Italic"
                            FontSize="12"
                            VerticalOptions="End" />
                    </Grid>
                </Frame>
            </DataTemplate>
        </CarouselView.ItemTemplate>
    </CarouselView>
</StackLayout>
```

[Android(carouselview-images/side-items.png "CarouselView side 항목") ![에서 CarouselView 항목을 사용 하는 스크린 샷]] (carouselview-images/side-items-large.png#lightbox "CarouselView side 항목")

## <a name="making-adjacent-items-partially-visible"></a>인접 항목이 부분적으로 표시 되도록 설정

앱에서 CarouselView를 사용 하는 경우 `PeekAreaInsets` 속성을 0이 아닌 값 (기본값)으로 설정 하 여 사용자에 게 CarouselView가 작동 하는 것을 사용자에 게 표시 하는 것이 유용할 수 있습니다. 이러한 값은 화면에서 부분적으로 노출 됩니다.

```xaml
<StackLayout Margin="20">
  <CarouselView ItemsSource="{Binding Monkeys}" HeightRequest="125" PeekAreaInsets="100">
      <CarouselView.ItemTemplate>
          <DataTemplate>
              <Frame BorderColor="Black">
                  <Grid>
                      <Grid.RowDefinitions>
                          <RowDefinition Height="Auto" />
                          <RowDefinition Height="Auto" />
                      </Grid.RowDefinitions>
                      <Grid.ColumnDefinitions>
                          <ColumnDefinition Width="Auto" />
                          <ColumnDefinition Width="Auto" />
                      </Grid.ColumnDefinitions>
                      <Label Grid.Column="1"
                          Text="{Binding Name}"
                          FontAttributes="Bold"
                          FontSize="14"/>
                      <Label Grid.Row="1"
                          Grid.Column="1"
                          Text="{Binding Location}"
                          FontAttributes="Italic"
                          FontSize="12"
                          VerticalOptions="End" />
                  </Grid>
              </Frame>
          </DataTemplate>
      </CarouselView.ItemTemplate>
  </CarouselView>
</StackLayout>
```

[Android(carouselview-images/peek-area-insets.png "CarouselView side 항목") ![에서 CarouselView 항목을 사용 하는 스크린 샷]] (carouselview-images/peek-area-insets-large.png#lightbox "CarouselView side 항목")

## <a name="related-links"></a>관련 링크

- [CollectionView (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CarouselViewDemos/)
- [CollectionView 레이아웃 설명서](../collectionview/layout.md)
