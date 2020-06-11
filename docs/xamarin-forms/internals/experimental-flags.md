---
제목: " Xamarin.Forms 실험적 플래그" 설명: " Xamarin.Forms 실험적 플래그를 사용 하면 엔지니어링 팀에서 사용자에 게 새로운 기능을 제공 하는 동시에 기능 api를 안정적인 릴리스로 전환 하기 전에 변경할 수 있습니다."
assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2: xamarin-forms author: davidbritch: dabritch:: 04/14/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-experimental-flags"></a>Xamarin.Forms실험적 플래그

새 Xamarin.Forms 기능이 구현 되 면 종종 실험적 플래그 뒤에 배치 됩니다. 이를 통해 엔지니어링 팀은 새로운 기능을 보다 신속 하 게 제공할 수 있으며, 기능 Api를 안정적인 릴리스로 전환 하기 전에 계속 해 서 변경할 수 있습니다. 그러면 기능이 안정적인 릴리스로 이동 되 면 실험적 플래그가 제거 됩니다.

Xamarin.Forms에는 다음 실험적 플래그가 포함 되어 있습니다.

- `AppTheme_Experimental`
- `CarouselView_Experimental`
- `Expander_Experimental`
- `IndicatorView_Experimental`
- `Markup_Experimental`
- `MediaElement_Experimental`
- `RadioButton_Experimental`
- `Shell_UWP_Experimental`
- `StateTriggers_Experimental`
- `SwipeView_Experimental`

실험적 플래그 뒤에 있는 기능을 사용 하려면 응용 프로그램에서 플래그 또는 플래그를 사용 하도록 설정 해야 합니다. 실험적 플래그를 사용 하는 방법에는 두 가지가 있습니다.

- 플랫폼 프로젝트에서 실험적 플래그 또는 플래그를 사용 하도록 설정 합니다.
- 클래스에서 실험적 플래그 또는 플래그를 사용 하도록 설정 합니다 `App` .

> [!WARNING]
> 플래그를 사용 하지 않고 실험적 플래그를 사용 하는 기능을 사용 하면 응용 프로그램에서 사용 해야 하는 플래그를 나타내는 예외를 throw 합니다.

## <a name="enable-flags-in-platform-projects"></a>플랫폼 프로젝트에서 플래그 사용

`Xamarin.Forms.Forms.SetFlags`메서드를 사용 하 여 플랫폼 프로젝트에서 실험적 플래그를 사용 하도록 설정할 수 있습니다.

```csharp
Xamarin.Forms.Forms.SetFlags("CarouselView_Experimental");
```

`SetFlags`메서드는 `AppDelegate` iOS의 클래스, Android의 클래스 `MainActivity` 및 UWP의 클래스에서 호출 해야 합니다 `App` .

> [!IMPORTANT]
> 플랫폼 프로젝트에서 실험적 플래그를 사용 하도록 설정 하는 `Forms.Init` 것은 메서드를 호출 하기 전에 수행 해야 합니다.

`Xamarin.Forms.Forms.SetFlags`메서드는 `string` 단일 메서드 호출에서 여러 실험적 플래그를 사용할 수 있도록 하는 배열 인수를 수락 합니다.

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "CarouselView_Experimental", "IndicatorView_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> `SetFlags`후속 호출이 이전 호출의 결과를 덮어쓰기 때문에 메서드를 두 번 이상 호출 하면 안 됩니다.

## <a name="enable-flags-in-your-app-class"></a>앱 클래스에서 플래그 사용

`Device.SetFlags`메서드를 사용 하 여 `App` 공유 코드 프로젝트의 클래스에서 실험적 플래그를 사용 하도록 설정할 수 있습니다.

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

`Device.SetFlags`메서드는 `IReadOnlyList<string>` 인수를 허용 하므로 단일 메서드 호출에서 여러 실험적 플래그를 사용 하도록 설정할 수 있습니다.

```csharp
Device.SetFlags(new string[]{ "CarouselView_Experimental", "MediaElement_Experimental", "SwipeView_Experimental" });
```

> [!WARNING]
> `SetFlags`후속 호출이 이전 호출의 결과를 덮어쓰기 때문에 메서드를 두 번 이상 호출 하면 안 됩니다.
