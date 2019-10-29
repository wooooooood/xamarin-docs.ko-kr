---
title: 코드를 Unified API로 업데이트하는 팁
description: 이 문서에서는 일반적인 오류 및 Xamarin의 Unified API 사용 하도록 응용 프로그램을 업데이트할 때 유용한 여러 가지 팁에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 8DD34D21-342C-48E9-97AA-1B649DD8B61F
ms.date: 03/29/2017
author: davidortinau
ms.author: daortin
ms.openlocfilehash: ee207ffc83f887e9c86c650b6f93fc2ff9f5b69c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014988"
---
# <a name="tips-for-updating-code-to-the-unified-api"></a>코드를 Unified API로 업데이트하는 팁

이전 Xamarin 솔루션을 Unified API로 업데이트 하는 경우 다음과 같은 오류가 발생할 수 있습니다.

## <a name="nsinvalidargumentexception-could-not-find-storyboard-error"></a>NSInvalidArgumentException에서 스토리 보드 오류를 찾을 수 없음

Mac용 Visual Studio의 현재 버전에는 자동화 된 마이그레이션 도구를 사용 하 여 프로젝트를 통합 된 Api로 변환 하는 동안 발생할 수 있는 [버그가](https://bugzilla.xamarin.com/show_bug.cgi?id=25569) 있습니다. 업데이트 후에 다음 형식으로 오류 메시지가 표시 되는 경우:

```console
Objective-C exception thrown. Name: NSInvalidArgumentException Reason: Could not find a storyboard named 'xxx' in bundle NSBundle...
```

다음 작업을 수행 하 여이 문제를 해결할 수 있습니다. 다음 빌드 대상 파일을 찾습니다.

```console
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/2.1/Xamarin.iOS.Common.targets
```

이 파일에서 다음 대상 선언을 찾아야 합니다.

```xml
<Target Name="_CopyContentToBundle"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

`DependsOnTargets="_CollectBundleResources"` 특성을 추가 합니다. 다음과 같습니다.

```xml
<Target Name="_CopyContentToBundle"
        DependsOnTargets="_CollectBundleResources"
        Inputs = "@(_BundleResourceWithLogicalName)"
        Outputs = "@(_BundleResourceWithLogicalName -> '$(_AppBundlePath)%(LogicalName)')" >
```

파일을 저장 하 고, Mac용 Visual Studio를 다시 부팅 하 고, 프로젝트를 새로 & 다시 빌드합니다. 이 문제에 대 한 픽스는 Xamarin에서 해제 해야 합니다.

## <a name="useful-tips"></a>유용한 팁

마이그레이션 도구를 사용한 후에도 수동으로 작업 해야 하는 일부 컴파일러 오류가 발생할 수 있습니다.
수동으로 수정 해야 할 수 있는 몇 가지 사항은 다음과 같습니다.

- `enum`를 비교할 때 `(int)` 캐스트가 필요할 수 있습니다.

- 이제 `NSDictionary.IntValue` `nint`를 반환 하 고 대신 사용할 수 있는 `Int32Value` 있습니다.

- `nfloat` 및 `nint` 형식은 `const`표시할 수 없습니다. `static readonly nint`은 적절 한 대안입니다.

- `MonoTouch.` 네임 스페이스에 직접 사용 되는 항목은 이제 `ObjCRuntime.` 네임 스페이스에 있습니다 (예: `MonoTouch.Constants.Version` 현재 `ObjCRuntime.Constants.Version`).

- 개체를 serialize 하는 코드는 `nint` 및 `nfloat` 형식을 serialize 하려고 할 때 중단 될 수 있습니다. Serialization 코드가 마이그레이션 후 예상 대로 작동 하는지 확인 해야 합니다.

- 경우에 따라 자동화 된 도구가 조건부 컴파일러 지시문 `#if #else` 내에서 코드를 누락 하 게 됩니다. 이 경우 수정 사항을 수동으로 설정 해야 합니다 (아래의 일반적인 오류 참조).

- `[Export]`를 사용 하 여 수동으로 내보낸 메서드는 마이그레이션 도구에 의해 자동으로 수정 되지 않을 수 있습니다. 예를 들어이 코드 snippert 반환 형식을 `nfloat`로 수동으로 업데이트 해야 합니다.

  ```csharp
  [Export("tableView:heightForRowAtIndexPath:")]
  public nfloat HeightForRow(UITableView tableView, NSIndexPath indexPath)
  ```

- Unified API는 무손실 변환이 아니기 때문에 NSDate와 .NET DateTime 간의 암시적 변환을 제공 하지 않습니다. `DateTimeKind.Unspecified`와 관련 된 오류를 방지 하려면 `NSDate`로 캐스팅 하기 전에 .NET `DateTime`를 local 또는 UTC로 변환 합니다.

- 이제는 목표-C 범주 메서드가 Unified API 확장 메서드로 생성 됩니다. 예를 들어 이전에 `UIView.DrawString` 사용한 코드는 이제 Unified API에서 `NSString.DrawString`를 참조 합니다.

- `VideoSettings`를 사용 하 여 AVFoundation 클래스를 사용 하는 코드는 `WeakVideoSettings` 속성을 사용 하도록 변경 해야 합니다. 이렇게 하려면 설정 클래스에서 속성으로 사용할 수 있는 `Dictionary`이 필요 합니다. 예를 들면 다음과 같습니다.

  ```csharp
  vidrec.WeakVideoSettings = new AVVideoSettings() { ... }.Dictionary;
  ```

- NSObject `.ctor(IntPtr)` 생성자가[잘못 된 사용을 방지 하기 위해](~/cross-platform/macios/unified/overview.md#NSObject_ctor)public에서 protected로 변경 되었습니다.

- `NSAction` 표준 .NET `Action`로 [대체](~/cross-platform/macios/unified/overview.md#NSAction) 되었습니다. 몇 가지 간단한 (단일 매개 변수) 대리자도 `Action<T>`으로 대체 되었습니다.

마지막으로, 코드에서 Api에 대 한 변경 내용을 조회 하려면 [클래식 v Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) 을 참조 하세요. [이 페이지](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) 를 검색 하면 클래식 api 및 업데이트 된 항목을 쉽게 찾을 수 있습니다.

> [!NOTE]
> `MonoTouch.Dialog` 네임 스페이스는 마이그레이션 후에도 동일 하 게 유지 됩니다. 코드에서 Monotouch.dialog를 사용 하는 경우 해당 네임 스페이스를 계속 사용 해야 합니다 **.** `Dialog``MonoTouch.Dialog` 변경 *하지* 마십시오.

## <a name="common-compiler-errors"></a>일반적인 컴파일러 오류

일반적인 오류의 기타 예는 솔루션과 함께 아래에 나열 되어 있습니다.

**오류 CS0012: ' Monotouch.dialog ' 형식이 참조 되지 않은 어셈블리에 정의 되어 있습니다.**

Fix: 일반적으로 프로젝트가 Unified API를 사용 하 여 빌드되지 않은 구성 요소 또는 NuGet 패키지를 참조 함을 의미 합니다. 모든 구성 요소와 NuGet 패키지를 삭제 하 고 다시 추가 해야 합니다. 이렇게 해도 오류가 해결 되지 않으면 외부 라이브러리에서 아직 Unified API을 지원 하지 않을 수 있습니다.

**오류 MT0034: ' monotouch.dialog ' 및 ' Xamarin.ios '를 모두 동일한 Xamarin.ios 프로젝트에 포함할 수 없습니다. ' Xamarin.ios '는 명시적으로 참조 되 고 ' monotouch.dialog '는 ' Xamarin.ios ', Version = 0.6.3.0, Culture = 중립에 의해 참조 됩니다. PublicKeyToken = null '.**

Fix:이 오류가 발생 하는 구성 요소를 삭제 하 고 프로젝트에 다시 추가 합니다.

**오류 CS0234: ' Monotouch.dialog ' 네임 스페이스에 ' Foundation ' 형식 또는 네임 스페이스 이름이 없습니다. 어셈블리 참조가 있나요?**

Fix: Mac용 Visual Studio의 자동화 된 마이그레이션 도구는 `Foundation`에 대 한 모든 `MonoTouch.Foundation` 참조를 업데이트 *해야* 하지만 일부 경우에는 수동으로 업데이트 해야 합니다. `UIKit`와 같이 `MonoTouch`에 이전에 포함 된 다른 네임 스페이스에도 유사한 오류가 나타날 수 있습니다.

**오류 CS0266: 암시적으로 ' double ' 형식을 ' m a s t '로 변환할 수 없습니다.**

Fix: type을 변경 하 고 `nfloat`로 캐스팅 합니다. 이 오류는 `nint`와 같은 64 비트의 다른 형식에도 발생할 수 있습니다.

```csharp
nfloat scale = (nfloat)Math.Min(rect.Width, rect.Height);
```

**오류 CS0266: ' CoreGraphics CGRect ' 형식을 ' RectangleF '로 암시적으로 변환할 수 없습니다. 명시적 변환이 있습니다 (캐스트를 누락 했습니까?).**

Fix: `CGRect``RectangleF` 인스턴스를 변경 하 고, `CGSize``SizeF` 하 고, `PointF`으로 변경 합니다. 네임 스페이스 `using System.Drawing;`은 `using CoreGraphics;` (이미 존재 하지 않는 경우)로 바꾸어야 합니다.

**오류 CS1502: ' CoreGraphics. CGContext (system.string, []) '에 대해 가장 적합 한 오버 로드 된 메서드 일치에 잘못 된 인수가 있습니다.**

Fix: 배열 형식을 `nfloat[]`로 변경 하 고 `Math.PI`명시적으로 캐스팅 합니다.

```csharp
grphc.SetLineDash (0, new nfloat[] { 0, 3 * (nfloat)Math.PI });
```

**오류 CS0115: ' WordsTableSource; int) '가 재정의로 표시 되어 있지만 재정의할 적절 한 메서드를 찾을 수 없습니다.**

Fix: 반환 값 및 매개 변수 형식을 `nint`로 변경 합니다. 이는 `UITableViewSource``RowsInSection`, `NumberOfSections`, `GetHeightForRow`, `TitleForHeader`, `GetViewForHeader`등의 메서드를 재정의할 때 주로 발생 합니다.

```csharp
public override nint RowsInSection (UITableView tableview, nint section) {
```

**오류 C S 0508: `WordsTableSource.NumberOfSections(UIKit.UITableView)`: 반환 형식은 재정의 된 멤버와 일치 해야 `UIKit.UITableViewSource.NumberOfSections(UIKit.UITableView)`**

Fix: 반환 형식이 `nint`변경 되 면 반환 값을 `nint`로 캐스팅 합니다.

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return (nint)navItems.Count;
}
```

**오류 CS1061: ' CoreGraphics. CGPath ' 형식에 ' AddElipseInRect '에 대 한 정의가 포함 되어 있지 않습니다.**

Fix: `AddEllipseInRect`철자를 수정 하십시오. 다른 이름 변경 내용은 다음과 같습니다.

- ' Color. Black '을 `NSColor.Black`로 변경 합니다.
- MapKit ' AddAnnotation '을 `AddAnnotations`변경 합니다.
- AVFoundation ' DataUsingEncoding '을 `Encode`로 변경 합니다.
- AVFoundation ' Avfoundation '을 `AVMetadataObjectType.QRCode`변경 합니다.
- AVFoundation ' 비디오 설정 '을 `WeakVideoSettings`로 변경 합니다.
- PopViewControllerAnimated을 `PopViewController`로 변경 합니다.
- CoreGraphics ' CGBitmapContext. SetRGBFillColor '를 `SetFillColor`로 변경 합니다.

**오류 CS0546: ' MapKit. MKAnnotation '에 재정의 가능한 set 접근자가 없으므로 재정의할 수 없습니다 (CS0546).**

MKAnnotation를 서브클래싱 하 여 사용자 지정 주석을 만들 때 좌표 필드에는 setter가 없고 getter만 있습니다.

[해결](https://forums.xamarin.com/discussion/comment/109505/#Comment_109505)방법:

- 필드를 추가 하 여 좌표 추적
- 좌표 속성의 getter에서이 필드를 반환 합니다.
- SetCoordinate 메서드를 재정의 하 고 필드를 설정 합니다.
- 전달 된 좌표 매개 변수를 사용 하 여 ctor에서 SetCoordinate를 호출 합니다.

결과는 다음과 비슷합니다.

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
- [IOS 앱 업데이트](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Mac 앱 업데이트](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin Forms 앱 업데이트](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [바인딩 업데이트](~/cross-platform/macios/unified/update-binding.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
