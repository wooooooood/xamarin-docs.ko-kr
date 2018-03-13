---
title: "장치 방향"
description: "레이아웃 가로 세로 방향에서 제대로 표시 하는 응용 프로그램을 지정 하는 방법을 이해 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: 8b266640bb0e1aa2bc584197e5fd7cbf4ab48e88
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="device-orientation"></a>장치 방향

응용 프로그램 사용 방법 및 사용자 환경을 개선 하기 위해 가로 방향을 통합 하는 방법을 고려 하는 것이 유용 합니다. 여러 방향을 수용 하기 위해 개별 레이아웃을 디자인할 수 있습니다 및 가장 사용 가능한 공간을 사용 합니다. 응용 프로그램 수준에서 회전 비활성화 되거나 활성화 될 수 있습니다.

이 문서 장치 방향 기능을 활용 하는 앱을 만드는 과정을 안내 하 고 다음과 같은 섹션이 있습니다.

- **[방향을 제어](#Controlling_Orientation)**  &ndash; 방향 앱 수준에서 각 플랫폼에서 제어 하는 방법을 이해 합니다.
- **[방향에서 변경 내용에 응답할](#Reacting_to_Changes_in_Orientation)**  &ndash; , 메시지가 표시 되며이에 대응 하는 방법을 알아보려면, 방향으로 변경 합니다.
- **[반응 형 레이아웃](#Responsive_Layout)**  &ndash; 가로 및 세로 방향에서 자동으로 작동 하는 레이아웃을 만드는 방법에 알아봅니다.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>방향을 제어합니다.

Xamarin.Forms를 사용 하면 지원 되는 방법은 장치 방향을 제어의 개별 프로젝트 각각에 대 한 설정을 사용 하도록 합니다.

> [!NOTE]
> Xamarin.Forms는 것을 방지 버그가 1.5.0가 기준으로 사용자 지정 렌더러 기반 실패 하는 방향을 제어 하려고 합니다. 참조 [이 토론](https://forums.xamarin.com/discussion/46653/forcing-landscape-for-a-single-page-in-ios#latest)자세한 정보에 대 한 Xamarin 포럼에서이 설명 합니다.

### <a name="ios"></a>iOS

장치 방향을 사용 하 여 응용 프로그램에 대해 구성 된 ios에서 **Info.plist** 파일입니다. 이 파일은 응용 프로그램을 대상으로 포함 하는 경우 iPhone 및 iPod에 대 한 방향 설정 뿐만 아니라 iPad에 대 한 설정을 포함 됩니다. IDE에 특정 지침은 다음과 같습니다. 이 문서의 맨 위에 있는 IDE 옵션을 사용 하 여 확인 하려는 어떤 명령을 선택:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 iOS 프로젝트를 열고 열기 **Info.plist**합니다. IPhone 배포 정보 탭부터 구성 패널로 파일이 열립니다.

![iPhone Visual Studio에서 배포 정보](device-orientation-images/orientation-vs-iphone.png)

IPad 방향을 구성 하려면 선택은 **iPad 배포 정보** 왼쪽 상단 패널의 다음 select는 사용 가능한 방향에서 탭:

![Visual Studio에서 지원 되는 장치 방향](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에서 iOS 프로젝트를 열고 열기 **Info.plist**합니다. 아래는 **응용 프로그램** 탭 섹션 방향을 설정 사용할 수 있습니다.

![iPhone Mac 용 Visual Studio에서 배포 정보](device-orientation-images/orientation-xam-ui.png)

선택 키-값 편집기 인터페이스를 사용 하 여 값을 편집 하려는 경우는 **소스**> 화면 아래쪽에서 탭:

![Mac 용 Visual Studio에서 장치 방향을 지원](device-orientation-images/orientation-xam-source.png)

-----


### <a name="android"></a>Android

Android에서 방향을 제어 하려면 열고 **MainActivity.cs** 데코레이팅하는 특성을 사용 하 여 방향을 설정의 `MainActivity` 클래스:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android는 방향을 지정 하기 위한 몇 가지 옵션을 지원 합니다.

- **가로** &ndash; 센서 데이터와 상관 없이 가로 되도록 응용 프로그램 입문을 강제로 수행 합니다.
- **세로** &ndash; 응용 프로그램 방향을 세로, 센서 데이터에 관계 없이 되도록 강제 합니다.
- **사용자** &ndash; 응용 프로그램이 사용자의 기본 설정된 방향이 사용 하 여 표시 될 수 있습니다.
- **뒤에 있는** &ndash; 의 방향으로 동일 하 게 응용 프로그램의 방향을 사용 하면는 [활동](https://developer.xamarin.com/api/type/Android.App.Activity/) 뒤 합니다.
- **센서** &ndash; 자동 회전을 비활성화 하는 경우에 사용 하면 응용 프로그램의 방향은 센서에 의해 결정 됩니다.
- **SensorLandscape** &ndash; 응용 가로 방향 (볼 수 있도록 화면 되지으로 180도 회전) 화면 향하도록 방향을 변경 하려면 센서 데이터를 사용 하는 동안을 사용 하도록 합니다.
- **SensorPortrait** &ndash; 응용 (볼 수 있도록 화면 되지으로 180도 회전) 화면 향하도록 방향을 변경 하려면 센서 데이터를 사용 하는 동안 세로 방향을 사용 하도록 합니다.
- **ReverseLandscape** &ndash; 가로 방향으로 반대 방향에서 일반적인 "180도 회전 합니다."를 표시 하기 위해 연결을 사용 하도록 응용 프로그램
- **ReversePortrait** &ndash; 세로 방향에서 일반적인 "180도 회전 합니다."를 표시 하기 위해 반대쪽으로 연결을 사용 하도록 응용 프로그램
- **FullSensor** &ndash; 응용 프로그램이 올바른 방향 (부족 가능한 4)을 선택 하는 센서 데이터를 사용 합니다.
- **FullUser** &ndash; 응용 프로그램이 사용자의 방향 기본 설정을 사용 합니다. 자동 회전 사용 하는 경우 모든 4 방향 사용할 수 있습니다.
- **UserLandscape** &ndash;  _\[지원 되지 않습니다\]_  인해 가로 방향으로 사용 하도록 응용 프로그램이 없을 경우 사용 하도록 설정 하는 자동 회전 사용할지는 쿼리에서 방향을 결정 하는 센서입니다. 이 옵션은 컴파일을 중단 됩니다.
- **UserPortrait** &ndash;  _\[지원 되지 않습니다\]_  없을 경우 사용 하도록 설정 하는 자동 회전 됩니다 사용할 경우 세로 방향을 사용 하 여 응용 프로그램이 방향을 결정 하는 센서입니다. 이 옵션은 컴파일을 중단 됩니다.
- **잠긴** &ndash;  _\[지원 되지 않습니다\]_  화면 방향을 사용 하 여 응용 프로그램이 무엇이 든, 실행 시 장치에 변경 내용에 응답 하지 않고의 실제 방향입니다. 이 옵션은 컴파일을 중단 됩니다.

네이티브 Android Api 많이 방향 관리 하는 방법에 대 한 제어를 제공 하는 참고를 명시적으로 사용자의 일치 하지 않는 옵션을 포함 하 여 기본 설정 표현 됩니다.

### <a name="windows-phone"></a>Windows Phone

지원 되는 방향에 설정 된 Windows Phone RT에서의 <span class="UIItem">Package.appxmanifest</span> 파일입니다. 매니페스트를 열어 지원 되는 방향을 선택할 수 있는 구성 패널을 표시 합니다.

![](device-orientation-images/vs-winrt-config.png "Package.appxmanifest Visual Editor")

Windows Phone 8 (Silverlight)에서 지원 되는 방향에는 코드에 설정 되어는 <span class="UIItem">MainPage.xaml.cs</span> 파일입니다. 기본 프로젝트 파일의 값이 이미 코드의 다음 행으로 설정 됩니다.

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
```

Windows Phone 방향 옵션을 지정 하려면 하는 원하는 방향 사용할 수 있도록 코드와 바꿉니다.

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
SupportedOrientations = SupportedPageOrientation.Portrait; // portrait only
SupportedOrientations = SupportedPageOrientation.Landscape; // landscape only
```

Windows Phone 지원 가로 뷰 모두에서 (처럼 세로에서) 왼쪽에서 오른쪽 및 오른쪽에서 왼쪽 방향입니다. 사용 되는 지정 하는 것이 불가능 합니다.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>방향에서 변경에 대응

Xamarin.Forms는 앱의 방향 변경 내용 공유 코드에 알리는 대 한 기본 이벤트를 제공 하지 않습니다. 그러나는 `SizeChanged` 의 이벤트는 `Page` 은 수행 된 경우 너비 또는 높이 `Page` 변경 합니다. 때의 너비는 `Page` 높이 보다 크면 장치가 가로 모드입니다. 자세한 내용은 참조 [화면 방향에 따라 이미지 표시](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)합니다.

> [!NOTE]
> 기존 무료 NuGet 패키지가 공유 코드에 방향 변경 알림을 수신 하기 위해 제공 됩니다. 참조는 [GitHub 리포지토리](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) 자세한 정보에 대 한 합니다.

재정의할 수는 또는 [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/) 메서드를 한 `Page`, 모든 레이아웃 삽입 그 논리를 변경 합니다. `OnSizeAllocated` 메서드를 호출 될 때마다는 `Page` 발생 하는 장치 회전 때도 항상 새 크기를 할당 합니다. 기본 구현을 `OnSizeAllocated` 재정의 기본 구현을 호출 해야 하므로 중요 한 레이아웃 기능을 수행 합니다.

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

해당 단계를 수행 하지 않으면 작동 하지 않는 페이지 발생 합니다.

`OnSizeAllocated` 메서드가 호출 될 수 여러 번 장치를 회전할 때. 레이아웃 될 때마다 변경 리소스의 낭비 이며 깜박임 발생할 수 있습니다. 페이지 내에서 인스턴스 변수를 사용 하 여 방향을 가로 또는 세로 인지 여부를 추적 하는 것이 좋습니다 하 고 변경 될 때 다시 그립니다.

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

장치 방향 변경 감지 되 면 추가 하거나 추가 보기에서 사용 가능한 공간 변경에 대응 하 여 사용자 인터페이스에서 제거 하는 것이 좋습니다. 예를 들어 세로 방향의 각 플랫폼에 기본 제공 계산기:

![](device-orientation-images/calculator-portrait.png "세로 방향의 계산기 응용 프로그램")

가로 및 세로:

![](device-orientation-images/calculator-landscape.png "가로 방향의 계산기 응용 프로그램")

앱 이용 사용 가능한 공간의 지형에 더 많은 기능을 추가 하 여 확인 합니다.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>반응 형 레이아웃

장치를 회전할 때 적절 하 게 전환 되도록 기본 제공 하는 레이아웃을 사용 하 여 디자인 인터페이스를 두는 것이 가능 합니다. 계속 변화 방향에 대응 하는 경우 매력적인 수 있는 인터페이스를 디자인할 때 고려 다음과 같은 일반 규칙.

- **비율에 주의 해야** &ndash; 방향에서 변경 비율을 관련 하 여 특정 가정을 사항이 있을 경우 문제가 발생할 수 있습니다. 예를 들어 세로 방향의 화면의 세로 간격의 1/3에 충분 한 공간이 있을 것 하는 보기 가로 세로 간격의 1/3에 적합 하지 않을 수 있습니다.
- **절대 값으로 주의 해야** &ndash; (픽셀) 절대 값을 제대로 세로 방향의 하면 안 됩니다 가로 방향의 합니다. 절대 값이 필요한 경우 해당 영향을 격리 하려면 중첩 된 레이아웃을 사용 합니다. 예를 들어는 것의 절대 값을 사용 하는 데는 `TableView` `ItemTemplate` 항목 템플릿을 uniform 높이 보장된 하는 경우.

위의 규칙 모범 사례 고려 일반적으로 한 화면 크기 및는 여러 인터페이스를 구현 하는 경우에 적용 됩니다. 이 가이드의 나머지 부분에서는 Xamarin.Forms에 기본 레이아웃의 각를 사용 하 여 반응 형 레이아웃의 구체적인 예제를 설명 합니다.

> [!NOTE]
> 다음 섹션에서는 이해를 돕기 위해 반응 형 레이아웃 형식 중 하나를 사용 하 여 구현 하는 방법을 보여 줍니다 `Layout` 한 번에 있습니다. 실제로 더 자주 간단 하 게 혼합 `Layout`더 간단한 클라이언트나 가장 직관적인를 사용 하 여 원하는 레이아웃을 달성 하기 위해 s `Layout` 각 구성 요소에 대 한 합니다.

### <a name="stacklayout"></a>StackLayout

세로에 표시 된 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-stack-portrait.png "세로 방향의 사진 응용 프로그램")

가로 및 세로:

![](device-orientation-images/photo-stack-landscape.png "가로 방향의 사진 응용 프로그램")

다음 XAML로 수행 됩니다.

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

일부 C#의 방향을 변경 사용은 `outerStack` 장치의 방향에 따라 다릅니다.

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

다음 사항에 유의하십시오.

- `outerStack` 사용 가능한 공간을 최대한 이용할 방향에 따라 가로 또는 세로 스택으로는 이미지와 컨트롤을 표시 하도록 조정 됩니다.


### <a name="absolutelayout"></a>AbsoluteLayout

세로에 표시 된 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-abs-portrait.png "세로 방향의 사진 응용 프로그램")

가로 및 세로:

![](device-orientation-images/photo-abs-landscape.png "가로 방향의 사진 응용 프로그램")

다음 XAML로 수행 됩니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
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

다음 사항에 유의하십시오.

- 페이지에 배치 된 방식으로 인해 응답성을 소개 하기 위한 프로시저 코드에 대 한 않아도가 됩니다.
- `ScrollView` 레이블을 보이지 않는 경우에 화면의 높이 단추와 이미지의 고정된 높이의 합계 보다 작을 수 있도록 사용 되는 합니다.


### <a name="relativelayout"></a>RelativeLayout

세로에 표시 된 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-rel-portrait.png "세로 방향의 사진 응용 프로그램")

가로 및 세로:

![](device-orientation-images/photo-rel-landscape.png "가로 방향의 사진 응용 프로그램")

다음 XAML로 수행 됩니다.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
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

다음 사항에 유의하십시오.

- 페이지에 배치 된 방식으로 인해 응답성을 소개 하기 위한 프로시저 코드에 대 한 않아도가 됩니다.
- `ScrollView` 레이블을 보이지 않는 경우에 화면의 높이 단추와 이미지의 고정된 높이의 합계 보다 작을 수 있도록 사용 되는 합니다.

### <a name="grid"></a>표

세로에 표시 된 다음 응용 프로그램을 고려 합니다.

![](device-orientation-images/photo-grid-portrait.png "세로 방향의 사진 응용 프로그램")

가로 및 세로:

![](device-orientation-images/photo-grid-landscape.png "가로 방향의 사진 응용 프로그램")

다음 XAML로 수행 됩니다.

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

함께 회전 변경 처리를 위해 다음 프로시저 코드:

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

다음 사항에 유의하십시오.

- 페이지에 배치 된 방식으로 인해 컨트롤의 창 배치를 변경 하는 방법이 있습니다.


## <a name="related-links"></a>관련 링크

- [레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 예제 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [반응 형 레이아웃 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [화면 방향을 기반으로 이미지를 표시 합니다.](https://developer.xamarin.comhttps://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
