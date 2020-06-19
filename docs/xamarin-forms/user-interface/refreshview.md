---
title: Xamarin.FormsRefreshView
description: Xamarin.FormsRefreshview는 스크롤 가능한 콘텐츠의 기능을 새로 고치는 기능을 제공 하는 컨테이너 컨트롤입니다.
ms.prod: xamarin
ms.assetId: 58DBD23B-ADB9-40DA-B331-4DDB6E698990
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/19/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d84e6bb6ed41f2fbc213cd15051d071521f588cd
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127597"
---
# <a name="xamarinforms-refreshview"></a>Xamarin.FormsRefreshView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)

는 `RefreshView` 스크롤 가능한 콘텐츠의 기능을 새로 고치기 위해 끌어오기를 제공 하는 컨테이너 컨트롤입니다. 따라서의 자식은 `RefreshView` , 또는와 같은 스크롤할 수 있는 컨트롤 이어야 합니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`CollectionView`](xref:Xamarin.Forms.CollectionView) [`ListView`](xref:Xamarin.Forms.ListView) .

`RefreshView`는 다음 속성을 정의합니다.

- `Command``ICommand`새로 고침이 트리거될 때 실행 되는 형식의입니다.
- `object` 형식의 `CommandParameter` - `Command`에 전달되는 매개 변수입니다.
- `IsRefreshing`의 `bool` 현재 상태를 나타내는 형식의입니다 `RefreshView` .
- `RefreshColor`형식의, `Color` 새로 고침 중에 표시 되는 진행률 원의 색입니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

> [!NOTE]
> 유니버설 Windows 플랫폼에서의 끌어오기 방향은 `RefreshView` 플랫폼별로 설정할 수 있습니다. 자세한 내용은 [Refreshview 끌어오기 방향](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)을 참조 하세요.

## <a name="create-a-refreshview"></a>RefreshView 만들기

다음 예제에서는 XAML로를 인스턴스화하는 방법을 보여 줍니다 `RefreshView` .

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <ScrollView>
        <FlexLayout Direction="Row"
                    Wrap="Wrap"
                    AlignItems="Center"
                    AlignContent="Center"
                    BindableLayout.ItemsSource="{Binding Items}"
                    BindableLayout.ItemTemplate="{StaticResource ColorItemTemplate}" />
    </ScrollView>
</RefreshView>
```

`RefreshView`코드에서를 만들 수도 있습니다.

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

ScrollView scrollView = new ScrollView();
FlexLayout flexLayout = new FlexLayout { ... };
scrollView.Content = flexLayout;
refreshView.Content = scrollView;
```

이 예제에서는 해당 자식이 인로의 `RefreshView` 새로 고침 기능을 제공 합니다 [`ScrollView`](xref:Xamarin.Forms.ScrollView) [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) . 는 바인딩 `FlexLayout` 가능한 레이아웃을 사용 하 여 항목 컬렉션에 바인딩하여 콘텐츠를 생성 하 고 각 항목의 모양을로 설정 합니다 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . 바인딩 가능한 레이아웃에 대 한 자세한 내용은 [ Xamarin.Forms 의 바인딩 가능한 레이아웃 ](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)을 참조 하세요.

속성의 값은 `RefreshView.IsRefreshing` 의 현재 상태를 나타냅니다 `RefreshView` . 사용자가 새로 고침을 트리거하는 경우이 속성은 자동으로로 전환 됩니다 `true` . 새로 고침이 완료 되 면 속성을로 다시 설정 해야 합니다 `false` .

사용자가 새로 고침을 시작 하면 `ICommand` 속성으로 정의 된가 `Command` 실행 되어 표시 되는 항목을 새로 고쳐야 합니다. 새로 고침이 발생 하는 동안 애니메이션 처리 원으로 구성 된 새로 고침 시각화가 표시 됩니다.

[![IOS 및 Android에서 데이터를 새로 고치는 RefreshView의 스크린샷](refreshview-images/default-progress-circle.png "RefreshView 데이터 새로 고침")](refreshview-images/default-progress-circle-large.png#lightbox "RefreshView 데이터 새로 고침")

> [!NOTE]
> `IsRefreshing`속성을로 수동으로 설정 `true` 하면 새로 고침 시각화가 트리거되고 `ICommand` 속성으로 정의 된이 실행 됩니다 `Command` .

## <a name="refreshview-appearance"></a>RefreshView 모양

클래스에서 상속 되는 속성 외에 `RefreshView` [`VisualElement`](xref:Xamarin.Forms.VisualElement) `RefreshView` 도에서 속성을 정의 합니다 `RefreshColor` . 이 속성을 설정 하 여 새로 고침 중에 표시 되는 진행률 원의 색을 정의할 수 있습니다.

```xaml
<RefreshView RefreshColor="Teal"
             ... />
```

다음 스크린샷에서는 속성이 설정 된를 보여 줍니다 `RefreshView` `RefreshColor` .

[![IOS 및 Android에서 청록 진행률 원이 있는 RefreshView의 스크린샷](refreshview-images/teal-progress-circle.png "청록 진행률 원이 있는 RefreshView")](refreshview-images/teal-progress-circle-large.png#lightbox "청록 진행률 원이 있는 RefreshView")

또한 `BackgroundColor` 속성을 진행률 원의 배경색을 나타내는로 설정할 수 있습니다 [`Color`](xref:Xamarin.Forms.Color) .

> [!NOTE]
> IOS에서 속성은 `BackgroundColor` `UIView` 진행률 원이 포함 된의 배경색을 설정 합니다.

## <a name="disable-a-refreshview"></a>RefreshView 사용 안 함

응용 프로그램은 끌어오기가 올바른 작업이 아닌 상태를 입력할 수 있습니다. 이러한 경우에는 `RefreshView` 속성을로 설정 하 여를 비활성화할 수 있습니다 `IsEnabled` `false` . 이렇게 하면 사용자가 끌어오기를 트리거하여 새로 고칠 수 없게 됩니다.

또는 속성을 정의할 때 `Command` `CanExecute` 의 대리자를 `ICommand` 지정 하 여 명령을 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [RefreshView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-refreshviewdemo/)
- [바인딩 가능한 레이아웃Xamarin.Forms](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
- [RefreshView 풀 방향 플랫폼 관련](~/xamarin-forms/platform/windows/refreshview-pulldirection.md)
