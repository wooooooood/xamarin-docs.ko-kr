---
title: 디바이스 방향
description: 이 문서에서는 Xamarin.Forms 세로 및 가로 방향으로 크게 보이는 응용 프로그램을 레이아웃 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b1a47d4dcc92fca4d280708a2cbbe9374c17da8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573302"
---
# <a name="device-orientation"></a>디바이스 방향

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

사용자 환경을 개선 하기 위해 응용 프로그램을 사용 하는 방법과 가로 방향을 통합 하는 방법을 고려 하는 것이 중요 합니다. 여러 방향을 수용 하 고 사용 가능한 공간을 최대한 활용 하도록 개별 레이아웃을 디자인할 수 있습니다. 응용 프로그램 수준에서 회전을 사용 하지 않거나 사용 하도록 설정할 수 있습니다.

## <a name="controlling-orientation"></a>방향 제어

를 사용 하는 경우 Xamarin.Forms 장치 방향을 제어 하는 지원 되는 방법은 각 개별 프로젝트에 대 한 설정을 사용 하는 것입니다.

### <a name="ios"></a>iOS

IOS에서 **info.plist** 파일을 사용 하 여 응용 프로그램에 대 한 장치 방향이 구성 됩니다. 이 문서의 맨 위에 있는 IDE 옵션을 사용 하 여 보려는 지침을 선택 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에서 iOS 프로젝트를 열고 **info.plist**을 엽니다. 이 파일은 iPhone 배포 정보 탭에서 시작 하 여 구성 패널로 열립니다.

![Visual Studio의 iPhone 배포 정보](device-orientation-images/orientation-vs.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio에서 iOS 프로젝트를 열고 **info.plist**을 엽니다. **응용 프로그램** 탭에서 방향을 설정 하는 데 섹션을 사용할 수 있습니다.

![Mac용 Visual Studio의 iPhone 배포 정보](device-orientation-images/orientation-vsmac.png)

키-값 편집기 인터페이스를 사용 하 여 값을 편집 하려면 화면 아래쪽에 있는 **원본**> 탭을 선택 합니다.

![Mac용 Visual Studio에서 지원 되는 장치 방향](device-orientation-images/orientation-source-vsmac.png)

-----

### <a name="android"></a>Android

Android에서 방향을 제어 하려면 **MainActivity.cs** 를 열고 클래스를 데코레이팅하는 특성을 사용 하 여 방향을 설정 합니다 `MainActivity` .

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin Android는 방향 지정을 위한 몇 가지 옵션을 지원 합니다.

- **가로** &ndash; 센서 데이터에 관계 없이 응용 프로그램 방향이 가로가 되도록 합니다.
- **세로** &ndash; 센서 데이터에 관계 없이 응용 프로그램 방향을 세로 방향으로 강제 합니다.
- **사용자** &ndash; 사용자의 기본 설정 된 방향을 사용 하 여 응용 프로그램이 표시 되도록 합니다.
- **뒤** &ndash; 응용 프로그램의 방향이 그 뒤의 [작업](xref:Android.App.Activity) 방향과 동일 하 게 합니다.
- **센서** &ndash; 사용자가 자동 회전을 사용 하지 않도록 설정한 경우에도 응용 프로그램의 방향이 센서에 의해 결정 됩니다.
- **SensorLandscape** &ndash; 응용 프로그램에서 센서 데이터를 사용 하 여 화면이 마주보는 방향 (화면이 거꾸로 표시 되지 않도록)을 변경 하는 동안 가로 방향을 사용 합니다.
- **SensorPortrait** &ndash; 응용 프로그램에서 센서 데이터를 사용 하 여 화면이 마주보는 방향 (화면이 거꾸로 표시 되지 않도록)을 변경 하는 동안 세로 방향을 사용 합니다.
- **ReverseLandscape** &ndash; 응용 프로그램이 일반적으로 반대 방향에 따라 가로 방향을 사용 하 여 "상하 대칭 이동" 하는 것 처럼 보이게 합니다.
- **ReversePortrait** &ndash; 응용 프로그램이 평소와 반대 방향으로 세로 방향을 사용 하 여 "상하 대칭 이동" 하는 것 처럼 보이게 합니다.
- **Fullsensor** &ndash; 응용 프로그램에서 센서 데이터를 사용 하 여 올바른 방향을 선택 하도록 합니다 (가능한 4 개 중에서 선택).
- **Fulluser** &ndash; 응용 프로그램에서 사용자의 방향 기본 설정을 사용 하도록 합니다. 자동 회전을 사용 하는 경우 4 방향 모두를 사용할 수 있습니다.
- **Userlandscape** &ndash; _ \[ 지원 \] 되지 않음_ 을 지정 하면 사용자가 자동 회전을 사용 하는 경우를 제외 하 고 응용 프로그램에서 가로 방향을 사용 합니다 .이 경우 센서를 사용 하 여 방향을 결정 하 게 됩니다. 이 옵션을 선택 하면 컴파일이 중단 됩니다.
- **Userportrait** &ndash; _ \[ 지원 \] 되지 않음_ 을 지정 하면 사용자가 자동 회전을 사용 하는 경우를 제외 하 고 응용 프로그램에서 세로 방향을 사용 합니다 .이 경우 센서를 사용 하 여 방향을 결정 하 게 됩니다. 이 옵션을 선택 하면 컴파일이 중단 됩니다.
- **잠김** &ndash; _ \[ 지원 \] 되지 않음_ 을 사용 하면 응용 프로그램에서 장치의 실제 방향 변경에 응답 하지 않고 화면 방향을 사용 하 여 시작 시와 동일 하 게 됩니다. 이 옵션을 선택 하면 컴파일이 중단 됩니다.

기본 Android Api는 사용자의 표현 된 기본 설정과 명확 하 게 일치 하지 않는 옵션을 포함 하 여 방향이 관리 되는 방식을 제어 합니다.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

UWP (유니버설 Windows 플랫폼)에서 지원 되는 방향은 **appxmanifest.xml** 파일에 설정 됩니다. 매니페스트를 열면 지원 되는 방향을 선택할 수 있는 구성 패널이 표시 됩니다.

## <a name="reacting-to-changes-in-orientation"></a>방향 변경에 대응

Xamarin.Forms는 공유 코드의 방향 변경을 응용 프로그램에 알리기 위한 네이티브 이벤트를 제공 하지 않습니다. 그러나에는 [Xamarin.Essentials](~/essentials/index.md) `DeviceDisplay` 방향 변경에 대 한 알림을 제공 하는 [] 클래스가 포함 되어 있습니다.

를 사용 하지 않고 방향을 감지 하려면의 Xamarin.Essentials `SizeChanged` `Page` 너비나 높이가 변경 될 때 발생 하는의 이벤트를 모니터링 합니다 `Page` . 의 너비가 `Page` 높이 보다 크면 장치가 가로 모드입니다. 자세한 내용은 [화면 방향에 따라 이미지 표시](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)를 참조 하세요.

또는에서 메서드를 재정의 하 여 [`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*) `Page` 레이아웃 변경 논리를 삽입할 수 있습니다. 메서드는가 `OnSizeAllocated` `Page` 장치를 회전할 때마다 발생 하는 새 크기가 할당 될 때마다 호출 됩니다. 의 기본 구현에서는 `OnSizeAllocated` 중요 한 레이아웃 함수를 수행 하므로 재정의에서 기본 구현을 호출 하는 것이 중요 합니다.

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

이 단계를 수행 하지 않으면 작동 하지 않는 페이지가 생성 됩니다.

`OnSizeAllocated`장치를 회전할 때 메서드를 여러 번 호출할 수 있습니다. 언제 든 지 레이아웃을 변경 하면 불필요 한 리소스를 사용할 수 있으며 깜박임이 발생할 수 있습니다. 페이지 내에서 인스턴스 변수를 사용 하 여 방향이 가로 또는 세로 인지 여부를 추적 하 고 변경 내용이 있을 때만 다시 그리도록 합니다.

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

장치 방향이 변경 되 면 사용 가능한 공간 변경에 대응할 수 있도록 사용자 인터페이스에 대 한 추가 보기를 추가 하거나 제거 하는 것이 좋습니다. 예를 들어 세로에서 각 플랫폼에 대 한 기본 제공 계산기를 살펴보겠습니다.

![](device-orientation-images/calculator-portrait.png "Calculator Application in Portrait")

및 가로:

![](device-orientation-images/calculator-landscape.png "Calculator Application in Landscape")

앱은 가로에서 더 많은 기능을 추가 하 여 사용 가능한 공간을 활용 합니다.

## <a name="responsive-layout"></a>반응 형 레이아웃

장치를 회전할 때 정상적으로 전환 되도록 기본 제공 레이아웃을 사용 하 여 인터페이스를 디자인할 수 있습니다. 방향 변경에 대응할 때 계속 해 서 유용 하 게 사용할 수 있는 인터페이스를 디자인 하는 경우 다음과 같은 일반 규칙을 고려 하십시오.

- **비율** &ndash; 에 주의 비율과 관련 하 여 특정 가정이 적용 될 경우 방향이 변경 되 면 문제가 발생할 수 있습니다. 예를 들어 세로 방향의 화면 세로 공간에 대 한 1/3의 공간이 충분 한 뷰는 가로 세로 공간의 1/3에 맞지 않을 수 있습니다.
- **절대 값** &ndash; 을 사용 하 여 주의 세로에서 의미가 있는 절대 (픽셀) 값은 가로에서 의미가 없습니다. 절대 값이 필요한 경우 중첩 된 레이아웃을 사용 하 여 해당 영향을 격리 합니다. 예를 들어 `TableView` `ItemTemplate` 항목 템플릿에 균일 한 높이가 보장 되는 경우에서 절대 값을 사용 하는 것이 합리적입니다.

위의 규칙은 여러 화면 크기에 대 한 인터페이스를 구현 하는 경우에도 적용 되며 일반적으로 최상의 방법으로 간주 됩니다. 이 가이드의 나머지 부분에서는의 각 기본 레이아웃을 사용 하 여 반응 형 레이아웃의 특정 예를 설명 합니다 Xamarin.Forms .

> [!NOTE]
> 명확 하 게 하기 위해 다음 섹션에서는 한 번에 한 가지 유형의만 사용 하 여 응답성이 뛰어난 레이아웃을 구현 하는 방법을 보여 줍니다 `Layout` . 실제로 `Layout` `Layout` 각 구성 요소에 대해 보다 간단 하거나 직관적으로를 사용 하 여 원하는 레이아웃을 확보 하는 것이 더 간단 합니다.

### <a name="stacklayout"></a>StackLayout

세로 방향으로 표시 되는 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-stack-portrait.png "Photo Application in Portrait")

및 가로:

![](device-orientation-images/photo-stack-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여이 작업을 수행 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

일부 c #은 `outerStack` 장치의 방향에 따라의 방향을 변경 하는 데 사용 됩니다.

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

다음 사항에 유의하세요.

- `outerStack`사용 가능한 공간을 최대한 활용 하기 위해 방향에 따라 이미지 및 컨트롤을 가로 또는 세로 스택으로 표시 하도록 조정 됩니다.

### <a name="absolutelayout"></a>AbsoluteLayout

세로 방향으로 표시 되는 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-abs-portrait.png "Photo Application in Portrait")

및 가로:

![](device-orientation-images/photo-abs-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여이 작업을 수행 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

다음 사항에 유의하세요.

- 페이지의 레이아웃 방식 때문에 응답성을 도입 하는 절차 코드는 필요 하지 않습니다.
- `ScrollView`화면의 높이가 단추 및 이미지의 고정 높이의 합계 보다 작은 경우에도를 사용 하 여 레이블을 표시할 수 있습니다.

### <a name="relativelayout"></a>RelativeLayout

세로 방향으로 표시 되는 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-rel-portrait.png "Photo Application in Portrait")

및 가로:

![](device-orientation-images/photo-rel-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여이 작업을 수행 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

다음 사항에 유의하세요.

- 페이지의 레이아웃 방식 때문에 응답성을 도입 하는 절차 코드는 필요 하지 않습니다.
- `ScrollView`화면의 높이가 단추 및 이미지의 고정 높이의 합계 보다 작은 경우에도를 사용 하 여 레이블을 표시할 수 있습니다.

### <a name="grid"></a>표

세로 방향으로 표시 되는 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-grid-portrait.png "Photo Application in Portrait")

및 가로:

![](device-orientation-images/photo-grid-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여이 작업을 수행 합니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

다음 절차적 코드와 함께 회전 변경을 처리 합니다.

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

다음 사항에 유의하세요.

- 페이지가 배치 된 방식 때문에 컨트롤의 모눈 배치를 변경 하는 메서드가 있습니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [응답성이 뛰어난 레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [화면 방향에 따라 이미지 표시](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
