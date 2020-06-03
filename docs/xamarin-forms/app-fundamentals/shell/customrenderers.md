---
title: 'Xamarin.Forms Shell 사용자 지정 렌더러' description: 'Xamarin.Forms 셸 애플리케이션은 다양한 셸 클래스가 공개하는 속성 및 메서드를 통해 세부적으로 사용자 지정할 수 있습니다. 그러나 더 정교한 플랫폼별 사용자 지정이 필요한 경우에는 셸 사용자 지정 렌더러를 만들 수도 있습니다.'
ms.prod: ms.assetid: ms.technology: author: ms.author: ms.date: no-loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-shell-custom-renderers"></a>Xamarin.Forms Shell 사용자 지정 렌더러

Xamarin.Forms Shell 애플리케이션의 이점 중 하나는 다양한 셸 클래스가 공개하는 속성 및 메서드를 통해 해당 모양과 동작을 세부적으로 사용자 지정할 수 있다는 것입니다. 그러나 더 정교한 플랫폼별 사용자 지정이 필요한 경우에는 셸 사용자 지정 렌더러를 만들 수도 있습니다. 다른 사용자 지정 렌더러와 마찬가지로, 다른 플랫폼에서 기본 동작을 허용하는 동안 셸 사용자 지정 렌더러를 하나의 플랫폼 프로젝트에만 추가하여 모양과 동작을 사용자 지정할 수 있습니다. 또는 다른 셸 사용자 지정 렌더러를 각 플랫폼 프로젝트에 추가하여 iOS 및 Android에서 모양과 동작을 사용자 지정할 수 있습니다.

iOS 및 Android에서는 `ShellRenderer` 클래스를 사용하여 셸 애플리케이션을 렌더링합니다. iOS에서 `ShellRenderer` 클래스는 `Xamarin.Forms.Platform.iOS` 네임스페이스에서 찾을 수 있습니다. Android에서 `ShellRenderer` 클래스는 `Xamarin.Forms.Platform.Android` 네임스페이스에서 찾을 수 있습니다.

셸 사용자 지정 렌더러를 만드는 프로세스는 다음과 같습니다.

1. `Shell` 클래스를 서브클래싱합니다. 이 작업은 이미 셸 애플리케이션에서 수행됩니다.
1. 서브클래싱된 `Shell` 클래스를 사용합니다. 이 작업은 이미 셸 애플리케이션에서 수행됩니다.
1. `ShellRenderer` 클래스에서 파생되는 사용자 지정 렌더러 클래스를 필요한 플랫폼에서 만듭니다.

## <a name="create-a-custom-renderer-class"></a>사용자 지정 렌더러 클래스 만들기

셸 사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. `ShellRenderer` 클래스의 서브클래스를 만듭니다.
1. 필요한 사용자 지정을 수행하도록 필요한 메서드를 재정의합니다.
1. `ShellRenderer` 서브클래스에 `ExportRendererAttribute`를 추가하여 셸 애플리케이션을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 렌더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 셸 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 기본 `ShellRenderer` 클래스가 사용됩니다.

`ShellRenderer` 클래스는 다음 재정의 가능한 메서드를 공개합니다.

| iOS | Android |
| --- | --- |
| `SetElementSize`<br />`CreateFlyoutRenderer`<br />`CreateNavBarAppearanceTracker`<br />`CreatePageRendererTracker`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellItemTransition`<br />`CreateShellSearchResultsRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTabBarAppearanceTracker`<br />`Dispose`<br />`OnCurrentItemChanged`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateBackgroundColor` | `CreateFragmentForPage`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellFlyoutRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTrackerForToolbar`<br />`CreateToolbarAppearanceTracker`<br />`CreateTabLayoutAppearanceTracker`<br />`CreateBottomNavViewAppearanceTracker`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`SwitchFragment`<br />`Dispose` |

`FlyoutItem` 및 `TabBar` 클래스는 `ShellItem` 클래스의 별칭이고 `Tab` 클래스는 `ShellSection` 클래스의 별칭입니다. 따라서 `FlyoutItem` 개체의 사용자 지정 렌더러를 만들 때는 `CreateShellItemRenderer` 메서드를 재정의하고 `Tab` 개체의 사용자 지정 렌더러를 만들 때는 `CreateShellSectionRenderer` 메서드를 재정의해야 합니다.

> [!IMPORTANT]
> iOS 및 Android에는 둘 다 `ShellSectionRenderer` 및 `ShellItemRenderer`와 같은 셸 렌더러 클래스가 있습니다. 그러나 이 추가 렌더러 클래스는 `ShellRenderer` 클래스에서 재정의를 통해 생성됩니다. 따라서 이 추가 렌더러 클래스의 동작을 사용자 지정하려면 해당 클래스를 서브클래싱하고 서브클래싱된 `ShellRenderer` 클래스에서 적절한 재정의로 서브클래스 인스턴스를 만들면 됩니다.

### <a name="ios-example"></a>iOS 예제

다음 코드 예제에서는 iOS의 경우 셸 애플리케이션의 탐색 모음에서 배경 이미지를 설정하는 서브클래싱된 `ShellRenderer`를 보여 줍니다.

```csharp
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.iOS.MyShellRenderer))]
namespace Xaminals.iOS
{
    public class MyShellRenderer : ShellRenderer
    {
        protected override IShellSectionRenderer CreateShellSectionRenderer(ShellSection shellSection)
        {
            var renderer = base.CreateShellSectionRenderer(shellSection);
            if (renderer != null)
            {
                (renderer as ShellSectionRenderer).NavigationBar.SetBackgroundImage(UIImage.FromFile("monkey.png"), UIBarMetrics.Default);
            }
            return renderer;
        }
    }
}
```

`MyShellRenderer` 클래스는 `CreateShellSectionRenderer` 메서드를 재정의하고 기본 클래스를 통해 생성된 렌더러를 검색합니다. 그런 다음, 렌더러를 반환하기 전에 탐색 모음에서 배경 이미지를 설정하여 렌더러를 수정합니다.

### <a name="android-example"></a>Android 예제

다음 코드 예제에서는 Android의 경우 셸 애플리케이션의 탐색 모음에서 배경 이미지를 설정하는 서브클래싱된 `ShellRenderer`를 보여 줍니다.

```csharp
using Android.Content;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.Droid.MyShellRenderer))]
namespace Xaminals.Droid
{
    public class MyShellRenderer : ShellRenderer
    {
        public MyShellRenderer(Context context) : base(context)
        {
        }

        protected override IShellToolbarAppearanceTracker CreateToolbarAppearanceTracker()
        {
            return new MyShellToolbarAppearanceTracker(this);
        }
    }
}
```

`MyShellRenderer` 클래스는 `CreateToolbarAppearanceTracker` 메서드를 재정의하고 `MyShellToolbarAppearanceTracker` 클래스의 인스턴스를 반환합니다. 다음 예제에서는 `ShellToolbarAppearanceTracker`에서 파생된 `MyShellToolbarAppearanceTracker` 클래스를 보여 줍니다.

```csharp
using Android.Support.V7.Widget;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

namespace Xaminals.Droid
{
    public class MyShellToolbarAppearanceTracker : ShellToolbarAppearanceTracker
    {
        public MyShellToolbarAppearanceTracker(IShellContext context) : base(context)
        {
        }

        public override void SetAppearance(Toolbar toolbar, IShellToolbarTracker toolbarTracker, ShellAppearance appearance)
        {
            base.SetAppearance(toolbar, toolbarTracker, appearance);
            toolbar.SetBackgroundResource(Resource.Drawable.monkey);
        }
    }
}
```

`MyShellToolbarAppearanceTracker` 클래스는 `SetAppearance` 메서드를 재정의하며 도구 모음에서 배경 이미지를 설정하여 도구 모음을 수정합니다.

> [!IMPORTANT]
> `ShellRenderer` 클래스에서 파생된 사용자 지정 렌더러에 `ExportRendererAttribute`를 추가하면 됩니다. 서브클래싱된 `ShellRenderer` 클래스를 통해 추가적인 서브클래싱된 셸 렌더러 클래스가 만들어집니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 사용자 지정 렌더러](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
