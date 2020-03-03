---
title: 디바이스 방향
description: 이 문서에서는 설명 세로 및 가로 방향에서 보기 좋게 레이아웃 Xamarin.Forms 응용 프로그램은 방법입니다.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: f7526b6cecebadd30e95718b7e537026a6557adf
ms.sourcegitcommit: f43d5ecafd19cbc5cce39201916a83927a34617a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78214995"
---
# <a name="device-orientation"></a>디바이스 방향

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

응용 프로그램 사용 방법 및 사용자 환경을 개선 하기 위해 가로 방향을 통합 하는 방법을 고려 하는 것이 반드시 합니다. 여러 화면 방향에 맞게 개별 레이아웃을 디자인할 수 있습니다 하 고 최적의 사용 가능한 공간을 사용 합니다. 응용 프로그램 수준에서 회전을 사용 하지 않도록 설정 되거나 활성화 될 수 있습니다.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>방향을 제어합니다.

Xamarin.Forms를 사용 하는 경우 장치 방향 제어에 대 한 지원 되는 메서드의 개별 프로젝트 각각에 대 한 설정을 사용 하는 것입니다.

### <a name="ios"></a>iOS

IOS에서 **info.plist** 파일을 사용 하 여 응용 프로그램에 대 한 장치 방향이 구성 됩니다. 이 파일은 앱을 대상으로 포함 하는 경우 iPhone 및 iPod에 대 한 방향 설정 뿐만 아니라 iPad에 대 한 설정을 포함 됩니다. 다음은 특정 IDE에 지시 합니다. 참조 하려고 하는 지침을 선택 하려면이 문서의 맨 위에 있는 IDE 옵션을 사용 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio에서 iOS 프로젝트를 열고 **info.plist**을 엽니다. IPhone 배포 정보 탭을 사용 하 여 시작 하는 구성 패널에는 파일이 열립니다.

![iPhone Visual Studio에서 배포 정보](device-orientation-images/orientation-vs-iphone.png)

IPad 방향을 구성 하려면 패널의 왼쪽 위에서 **Ipad 배포 정보** 탭을 선택 하 고 사용 가능한 방향에서 선택 합니다.

![Visual Studio에서 지원 되는 장치 방향](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Mac용 Visual Studio에서 iOS 프로젝트를 열고 **info.plist**을 엽니다. **응용 프로그램** 탭에서 방향을 설정 하는 데 섹션을 사용할 수 있습니다.

![iPhone Mac 용 Visual Studio에서 배포 정보](device-orientation-images/orientation-xam-ui.png)

키-값 편집기 인터페이스를 사용 하 여 값을 편집 하려면 화면 아래쪽에 있는 **원본**> 탭을 선택 합니다.

![Mac 용 Visual Studio에서 장치 방향 지원](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Android에서 방향을 제어 하려면 **MainActivity.cs** 을 열고 `MainActivity` 클래스를 데코레이팅하는 특성을 사용 하 여 방향을 설정 합니다.

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android는 방향을 지정 하기 위한 몇 가지 옵션을 지원 합니다.

- **가로** &ndash; 센서 데이터에 관계 없이 응용 프로그램 방향이 가로가 되도록 합니다.
- **세로** &ndash;는 센서 데이터에 관계 없이 응용 프로그램 방향을 세로 방향으로 만듭니다.
- **사용자** &ndash; 사용자의 기본 설정 된 방향을 사용 하 여 응용 프로그램을 표시 합니다.
- &ndash; **이면** 응용 프로그램의 방향이 그 뒤의 [작업](xref:Android.App.Activity) 방향과 동일 하 게 됩니다.
- **센서** &ndash;는 사용자가 자동 회전을 사용 하지 않도록 설정한 경우에도 센서에 의해 응용 프로그램의 방향이 결정 됩니다.
- **SensorLandscape** &ndash;를 사용 하면 응용 프로그램에서 센서 데이터를 사용 하 여 화면이 마주보는 방향을 변경할 수 있습니다 (화면이 거꾸로 표시 되지 않음).
- **SensorPortrait** &ndash;를 사용 하면 응용 프로그램에서 센서 데이터를 사용 하는 동안 세로 방향을 사용 하 여 화면에 표시 되는 방향을 변경할 수 있습니다 (화면이 거꾸로 표시 되지 않음).
- **ReverseLandscape** &ndash;를 사용 하면 응용 프로그램에서 평상시와 반대 방향을 사용 하 여 가로 방향을 사용 하 여 "상하 다운" 하는 것 처럼 보입니다.
- **ReversePortrait** &ndash;를 사용 하면 응용 프로그램에서 평소와 반대 방향으로 세로 방향을 사용 하 여 "상하 대칭 이동" 하는 것 처럼 보입니다.
- **Fullsensor** &ndash;를 사용 하면 응용 프로그램에서 센서 데이터를 사용 하 여 올바른 방향 (4 ~ 4)을 선택할 수 있습니다.
- **Fulluser** &ndash;을 사용 하면 응용 프로그램에서 사용자의 방향 기본 설정을 사용 합니다. 자동 회전을 사용 하는 경우 모든 4 방향 사용할 수 있습니다.
- 사용자가 자동 회전을 사용 하는 경우를 제외 하 고 사용자가 자동 회전을 사용 하도록 설정 하는 경우를 제외 하 고, 사용자가 자동 회전을 사용 하는 경우를 제외 하 고는 _\]\[_ &ndash; 사용자의 **가로** 방향 이 옵션은 컴파일을 중단 됩니다.
- **Userportrait** &ndash; _\[\]지원 되지 않으므로_ 사용자가 자동 회전을 사용 하도록 설정 하지 않은 경우 응용 프로그램이 세로 방향을 사용 하 게 됩니다 .이 경우 센서를 사용 하 여 방향을 결정 합니다. 이 옵션은 컴파일을 중단 됩니다.
- **잠긴** &ndash; _\[지원 되지 않습니다.\]_ 를 사용 하면 장치의 실제 방향 변경에 응답 하지 않고 응용 프로그램에서 시작 시의 화면 방향을 사용 하 게 됩니다. 이 옵션은 컴파일을 중단 됩니다.

네이티브 Android Api는 많은 방향을 관리 하는 방법에 대 한 제어를 제공 하는 참고를 명시적으로 사용자의 일치 하지 않는 옵션을 포함 한 기본 표현 됩니다.

### <a name="universal-windows-platform"></a>유니버설 Windows 플랫폼

UWP (유니버설 Windows 플랫폼)에서 지원 되는 방향은 **appxmanifest.xml** 파일에 설정 됩니다. 매니페스트를 열어 지원 되는 방향을 선택할 수 있는 구성 패널을 표시 됩니다.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>방향에서 변경에 대응

Xamarin.Forms 앱 공유 코드의 방향 변경 내용을 알리는 대 한 기본 이벤트를 제공 하지 않습니다. 그러나[xamarin.ios](~/essentials/index.md) 는 방향 변경에 대 한 알림을 제공 하는 [`DeviceDisplay`] 클래스를 포함 합니다.

Xamarin.ios가 없는 방향을 검색 하려면 `Page`의 너비나 높이가 변경 될 때 발생 하는 `Page`의 `SizeChanged` 이벤트를 모니터링 합니다. `Page`의 너비가 높이 보다 크면 장치가 가로 모드입니다. 자세한 내용은 [화면 방향에 따라 이미지 표시](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)를 참조 하세요.

또는 `Page`에 대 한 [`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*) 메서드를 재정의 하 여 레이아웃 변경 논리를 삽입할 수 있습니다. `OnSizeAllocated` 메서드는 장치를 회전할 때마다 발생 하는 `Page` 새 크기가 할당 될 때마다 호출 됩니다. `OnSizeAllocated`의 기본 구현에서는 중요 한 레이아웃 함수를 수행 하므로 재정의에서 기본 구현을 호출 하는 것이 중요 합니다.

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

해당 단계를 수행 하는 오류는 작동 하지 않는 페이지에 발생 합니다.

장치를 회전할 때 `OnSizeAllocated` 메서드를 여러 번 호출할 수 있습니다. 레이아웃 될 때마다 변경 리소스의 낭비 이며 깜박임 발생할 수 있습니다. 페이지 내 인스턴스 변수를 사용 하 여 방향을 가로 또는 세로 인지 여부를 추적 하는 것이 좋습니다 하 고 변경 될 때 다시 그립니다.

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

장치 방향 변경에 감지 되 면 추가 또는 사용 가능한 공간 변경에 반응 하 여 사용자 인터페이스에서 추가 뷰를 제거 하는 것이 좋습니다. 예를 들어 세로에서 각 플랫폼에 기본 제공 계산기를 고려 합니다.

![](device-orientation-images/calculator-portrait.png "Calculator Application in Portrait")

가로 및 세로:

![](device-orientation-images/calculator-landscape.png "Calculator Application in Landscape")

앱 환경에서 더 많은 기능을 추가 하 여 사용 가능한 공간을 활용 하기 위해 있는지 확인 합니다.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>반응 형 레이아웃

디자인 인터페이스 장치를 회전할 때 정상적으로 전환 되도록 기본 제공 레이아웃을 사용 하는 것이 가능 합니다. 계속 방향에서 변경에 응답 하는 경우 매력적인 수 있는 인터페이스를 디자인할 때 다음 일반 규칙을 고려 합니다.

- 비율을 고려 &ndash; 하 여 비율에 대 한 **주의** 를 기울여야 합니다. 비율에 따라 특정 가정이 적용 되는 경우 문제가 발생할 수 있습니다. 예를 들어, 1/3의 세로 화면 세로 공간이 충분 한 공간에 있는 보기 1/3의 가로 세로 공간에 맞지 않을 수 있습니다.
- 세로에서 의미가 있는 절대 (픽셀) 값 &ndash;는 **절대 값을 사용** 하는 것이 적합 하지 않을 수 있습니다. 절대 값이 필요한 경우 그에 따른 영향을 격리 하려면 중첩 된 레이아웃을 사용 합니다. 예를 들어 항목 템플릿에 균일 한 높이가 보장 되는 경우 `TableView` `ItemTemplate`에서 절대 값을 사용 하는 것이 합리적입니다.

위의 규칙 모범 사례를 고려 일반적으로 여러 화면 크기에는 인터페이스를 구현 하는 경우에 적용 됩니다. 이 가이드의 나머지 부분에서는 Xamarin.Forms의 각 기본 레이아웃을 사용 하 여 반응 형 레이아웃의 특정 예제를 설명 합니다.

> [!NOTE]
> 명확성을 위해 다음 섹션에서는 한 번에 한 가지 유형의 `Layout`를 사용 하 여 응답성이 뛰어난 레이아웃을 구현 하는 방법을 보여 줍니다. 실제로 각 구성 요소에 대해 보다 간단 하 고 직관적인 `Layout`를 사용 하 여 원하는 레이아웃을 얻기 위해 `Layout`를 혼합 하는 것이 더 간단 합니다.

### <a name="stacklayout"></a>StackLayout

세로에 표시 된 다음 응용 프로그램을 고려해 야 합니다.

![](device-orientation-images/photo-stack-portrait.png "Photo Application in Portrait")

가로 및 세로:

![](device-orientation-images/photo-stack-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여 수행 됩니다.

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

일부 C# 는 장치의 방향에 따라 `outerStack` 방향을 변경 하는 데 사용 됩니다.

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

유의 사항은 다음과 같습니다.

- 사용 가능한 공간을 최대한 활용 하기 위해 방향에 따라 이미지와 컨트롤을 가로 또는 세로 스택으로 표시 하도록 `outerStack` 조정 됩니다.

### <a name="absolutelayout"></a>AbsoluteLayout

세로에 표시 된 다음 응용 프로그램을 고려해 야 합니다.

![](device-orientation-images/photo-abs-portrait.png "Photo Application in Portrait")

가로 및 세로:

![](device-orientation-images/photo-abs-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여 수행 됩니다.

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

유의 사항은 다음과 같습니다.

- 페이지에 배치 된 방식으로 인해 응답성을 소개 하는 절차적 코드에 대 한 않아도가 됩니다.
- 화면의 높이가 단추 및 이미지의 고정 높이의 합계 보다 작은 경우에도 `ScrollView` 사용 하 여 레이블을 표시할 수 있습니다.

### <a name="relativelayout"></a>RelativeLayout

세로에 표시 된 다음 응용 프로그램을 고려해 야 합니다.

![](device-orientation-images/photo-rel-portrait.png "Photo Application in Portrait")

가로 및 세로:

![](device-orientation-images/photo-rel-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여 수행 됩니다.

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

유의 사항은 다음과 같습니다.

- 페이지에 배치 된 방식으로 인해 응답성을 소개 하는 절차적 코드에 대 한 않아도가 됩니다.
- 화면의 높이가 단추 및 이미지의 고정 높이의 합계 보다 작은 경우에도 `ScrollView` 사용 하 여 레이블을 표시할 수 있습니다.

### <a name="grid"></a>모눈

세로에 표시 된 다음 응용 프로그램을 고려해 야 합니다.

![](device-orientation-images/photo-grid-portrait.png "Photo Application in Portrait")

가로 및 세로:

![](device-orientation-images/photo-grid-landscape.png "Photo Application in Landscape")

다음 XAML을 사용 하 여 수행 됩니다.

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

회전 변경을 처리할 수 있도록 다음 절차적 코드와 함께

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

유의 사항은 다음과 같습니다.

- 페이지에 배치 된 방식으로 인해 컨트롤의 눈금 위치를 변경 하는 방법이 있습니다.

## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble 예제 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [응답성이 뛰어난 레이아웃 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [화면 방향에 따라 이미지 표시](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
