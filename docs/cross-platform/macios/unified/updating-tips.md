---
title: "통합 된 API에 코드를 업데이트 하기 위한 팁"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 31ee94b7a463a651556472967152b98ee3061a8d
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>통합 된 API에 코드를 업데이트 하기 위한 팁

Mac은 iOS 및 Mac 프로젝트는 통합 API (Xamarin.iOS.dll 또는 Xamarin.Mac.dll)를 참조 하 고 많이 필요한 코드 변경 내용 쉽게 변환 됩니다에 대 한 Visual Studio에서 자동화 된 마이그레이션 도구를 사용 하 여 (참조는 [Xamarin Studio 릴리스 참고 사항 섹션 '통합 API 코드 마이그레이션 도구를 사용 하는 기본'](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/) 특정 세부 정보에 대 한).

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>NSInvalidArgumentException 찾을 수 없습니다 스토리 보드 오류

한 [버그](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) 자동된 마이그레이션 도구를 사용 하 여 Api를 통합 하 여 프로젝트를 변환 후 발생할 수 있는 Mac 용 Visual Studio의 현재 버전에서입니다. 오류 메시지가 형식에서 발생 하면 업데이트 후:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...

```

다음 빌드 대상 파일을 찾을이 문제를 해결 하려면 다음을 수행할 수 있습니다.

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

이 파일에 다음 대상 선언을 찾아야 할:

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

추가 `DependsOnTargets="_CollectBundleResources"` 특성을 합니다. 다음과 같습니다.

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

파일을 저장, Mac 용 Visual Studio를 다시 부팅 및 정리를 수행 및 프로젝트를 다시 작성 합니다. 이 문제에 대 한 수정 곧 Xamarin으로 해제 해야 합니다.

## <a name="useful-tips"></a>유용한 팁

마이그레이션 도구를 사용한 후 수동 개입이 필요 일부 컴파일러 오류가 여전히 발생할 수 있습니다.
수동으로 수정 해야 할 수 있는 몇 가지 사항은 다음과 같습니다.

* 비교 `enum`s 필요할 수 있습니다는 `(int)` 캐스트 합니다.

* `NSDictionary.IntValue` 이제 반환는 `nint`는 `Int32Value` 대신 사용할 수 있는 합니다.

* `nfloat` 및 `nint` 형식을 표시할 수 없습니다. `const`;   `static readonly nint` 합리적인 방법이 있습니다.

* 직접 되어야 하는 데 사용 하는 것은 `MonoTouch.` 네임 스페이스에 일반적으로 이제는 `ObjCRuntime.` 네임 스페이스 (예:: `MonoTouch.Constants.Version` 이제 `ObjCRuntime.Constants.Version`).

* Serialize 하려고 할 때 개체를 serialize 하는 코드가 중단 될 수 `nint` 및 `nfloat` 형식입니다. 마이그레이션 후 예상 대로 작동 하는 serialization 코드를 확인 해야 합니다.

* 자동화 도구 내에서 코드를 누락 하는 경우에 따라 `#if #else` 조건부 컴파일러 지시문입니다. 수정 프로그램을 수동으로 수행 해야 하는 경우 (일반적인 오류 아래 참조).

* 수동으로 내보낸된 메서드를 사용 하 여 `[Export]` 에서 반환 형식이 수동으로 업데이트 해야이 코드 snippert의 예를 들어 마이그레이션 도구는 자동으로 해결 하지 않을 수 있습니다 `nfloat`:

    ```csharp
    [Export("tableView:heightForRowAtIndexPath:")]
    public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
    ```

 * 무손실 변환이 없기 때문에 통합 API NSDate 및.NET DateTime 간에 암시적 변환이 제공 하지 않습니다. 와 관련 된 오류를 방지 하려면 `DateTimeKind.Unspecified` .NET 변환 `DateTime` 로컬 또는 UTC로 캐스팅 하기 전에 `NSDate`합니다.

 * Objective C 범주 메서드는 이제 통합 API에서 확장 메서드로 생성 됩니다. 예를 들어 이전에 사용한 코드 `UIView.DrawString` 은 이제 참조 `NSString.DrawString` 통합 API에서 합니다.

 * 사용 하 여 AVFoundation 클래스를 사용 하는 코드 `VideoSettings` 사용 하도록 변경 해야는 `WeakVideoSettings` 속성입니다. 이 위해서는 `Dictionary`, 사용 하지 않는 설정 클래스에서 속성으로 예:

    ```csharp
    vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
    ```

 * NSObject `.ctor(IntPtr)` 생성자에서 변경 된 공용으로 보호 된 ([부적절 하 게 사용 하지 않으려면](~/cross-platform/macios/unified/index.md#NSObject_ctor)).

 * `NSAction` 되었습니다 [대체](~/cross-platform/macios/unified/index.md#NSAction) starndard.NET와 `Action`합니다. 또한 몇 가지 간단한 (단일 매개 변수) 대리자로 대체 되었습니다 `Action<T>`합니다.

마지막으로, 참조는 [클래식 v 통합 API 차이점](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) 코드에서 Api의 변경 사항을 조회 하 합니다. 검색 [이 페이지](http://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/) 클래식 Api에 대 한 어떤 했으므로 된 업데이트를 찾을 하는 데 도움이 됩니다.

**참고:** 는 `MonoTouch.Dialog` 네임 스페이스 마이그레이션 후 동일 하 게 유지 합니다. 코드에서 **MonoTouch.Dialog** 수행-해당 네임 스페이스를 사용 하 여 계속 해야 *하지* 변경 `MonoTouch.Dialog` 를 `Dialog`!

## <a name="common-compiler-errors"></a>일반적인 컴파일러 오류

일반적인 오류에 대 한 다른 예제는 솔루션과 함께, 다음과 같습니다.

**오류 CS0012: 'MonoTouch.UIKit.UIView' 형식이 참조 되지 않은 어셈블리에 정의 됩니다.**

Fix:이 일반적으로 의미는 구성 요소 또는 통합 API로 작성 되지 않은 NuGet 패키지를 프로젝트에서 참조 합니다. 삭제 하 고 다시 모든 구성 요소와 NuGet 추가 패키지 합니다. 이 오류를 해결 하지 않는 경우 외부 라이브러리는 통합 API를 아직 지원 하지 않습니다.

**오류 MT0034: 동일한 Xamarin.iOS 프로젝트-에 'monotouch.dll'와 'Xamarin.iOS.dll'를 포함할 수 없습니다 'monotouch.dll'에서 참조 하는 동안 'Xamarin.iOS.dll' 명시적으로 참조 되 ' Xamarin.Mobile, Version = 0.6.3.0, Culture = neutral, PublicKeyToken = null'.**

Fix:이 오류를 발생 시키는 구성 요소를 삭제 하 고 다시 프로젝트에 추가 합니다.

**오류 CS0234: 형식 또는 네임 스페이스 이름 'Foundation' 'MonoTouch' 네임 스페이스에 존재 하지 않습니다. 어셈블리 참조가 없습니다?**

Fix: Mac 용 Visual Studio에서 자동화 된 마이그레이션 도구 *해야* 모두 업데이트 `MonoTouch.Foundation` 에 대 한 참조 `Foundation`있지만 경우에 따라 수동으로 업데이트 해야 합니다. 에 포함 된 이전에 다른 네임 스페이스에 대 한 유사한 오류가 나타날 수 `MonoTouch`와 같은 `UIKit`합니다.

**오류 CS0266: 'System.float'를 ' double' 형식에 암시적으로 변환할 수 없습니다.**

Fix: 유형을 변경 하 고 캐스팅 `nfloat`합니다. 64 비트 버전이 포함 된 다른 형식에 대 한도이 오류가 발생할 수 있습니다 (예: `nint`,)

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**오류 CS0266: 형식 'CoreGraphics.CGRect' 'System.Drawing.RectangleF'를 암시적으로 변환할 수 없습니다. 명시적 변환이 (없습니다 캐스트?)**

Fix:에 인스턴스를 변경 하 `RectangleF` 를 `CGRect`, `SizeF` 를 `CGSize`, 및 `PointF` 를 `CGPoint`합니다. 네임 스페이스 `using System.Drawing;` 로 대체 해야 `using CoreGraphics;` (이미 존재 하지) 하는 경우.

**오류 CS1502:는 가장에 대 한 일치 하는 메서드에 오버 로드 된 ' CoreGraphics.CGContext.SetLineDash (System.nfloat, System.nfloat[])'에 잘못 된 인수가 있습니다.**

Fix: 배열 형식을 변경 하 `nfloat[]` 명시적으로 캐스팅 하 고 `Math.PI`합니다.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**오류 CS0115: 'WordsTableSource.RowsInSection (UIKit.UITableView, int)'는 재정의 하지만 적절 한 방법을 재정의할를 찾을 수로 표시**

Fix: 반환 값 및 매개 변수 형식을 변경 `nint`합니다. 일반적으로 메서드 재정의 등으로 수행 `UITableViewSource`를 포함 하 여 `RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`등입니다.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**Error CS0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)': return type must be 'System.nint' to match overridden member `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)'**

Fix: 반환 형식으로 변경 된 경우 `nint`, 반환 값을 캐스팅 `nint`합니다.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**오류 CS1061: 'CoreGraphics.CGPath' 형식을 'AddElipseInRect'에 대 한 정의 포함 하지 않습니다.**

Fix: 맞춤법에 `AddEllipseInRect`합니다. 다른 이름 변경은 다음과 같습니다.

* 'Color.Black' 변경 `NSColor.Black`합니다.
* 'AddAnnotation' MapKit 변경 `AddAnnotations`합니다.
* 'DataUsingEncoding' AVFoundation 변경 `Encode`합니다.
* 'AVMetadataObject.TypeQRCode' AVFoundation 변경 `AVMetadataObjectType.QRCode`합니다.
* 'VideoSettings' AVFoundation 변경 `WeakVideoSettings`합니다.
* 변경에 PopViewControllerAnimated `PopViewController`합니다.
* 'CGBitmapContext.SetRGBFillColor' CoreGraphics 변경 `SetFillColor`합니다.

**'MapKit.MKAnnotation.Coordinate'에 재정의 가능한 set 접근자 (c s 0546) 없기 때문에 오류 CS0546: 재정의할 수 없습니다.**

사용자 지정 주석 MKAnnotation 서브클래싱하 면 만들 때 좌표 필드에 setter, getter에만 있습니다.

[해결](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505):

* 시작 하는 좌표를 추적 하기 위해 필드 추가
* 이 필드의 좌표 속성 getter의 반환
* SetCoordinate 메서드를 재정의 하 고 필드를 설정 합니다.
* 좌표에 전달 된 매개 변수를 사용 하면 ctor에 SetCoordinate 호출

에 다음과 같아야 합니다.

```csharp
class BasicPinAnnotation : MKAnnotation
{
    private CLLocationCoordinate2D _coordinate;

    public override CLLocationCoordinate2D Coordinate
    {
        get
        {
            return _coordinate;
        }
    }

    public override void SetCoordinate(CLLocationCoordinate2D value)
    {
        _coordinate = value;
    }

    public BasicPinAnnotation (CLLocationCoordinate2D coordinate)
    {
        SetCoordinate(coordinate);
    }
}
```

## <a name="related-links"></a>관련 링크

- [앱 업데이트](~/cross-platform/macios/unified/updating-apps.md)
- [IOS 앱을 업데이트합니다.](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms 앱 업데이트](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [바인딩 업데이트](~/cross-platform/macios/unified/update-binding.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
