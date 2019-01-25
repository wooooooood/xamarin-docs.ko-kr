---
title: Xamarin.iOS에서 시스템을 계산 하는 새 참조
description: 이 문서에서는 Xamarin의 향상 된 참조 시스템에 기본적으로 모든 Xamarin.iOS 응용 프로그램에서 사용을 계산을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 11/25/2015
ms.openlocfilehash: 8c7b1a88284156cb5d4261f18d5659ed66dfaf64
ms.sourcegitcommit: ee626f215de02707b7a94ba1d0fa1d75b22ab84f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54879331"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Xamarin.iOS에서 시스템을 계산 하는 새 참조

Xamarin.iOS 9.2.1 기본적으로 모든 응용 프로그램에 대 한 시스템 계산 향상 된 참조를 도입 합니다. 추적 하 고 이전 버전의 Xamarin.iOS 수정할 어려웠던 많은 메모리 문제를 제거 하기 위해 사용할 수 있습니다.

## <a name="enabling-the-new-reference-counting-system"></a>새 참조 횟수 시스템을 사용 하도록 설정

새 참조 시스템을 계산 하도록 Xamarin 9.2.1 기준 **모든** 기본적으로 응용 프로그램입니다.

기존 응용 프로그램을 개발 하는 경우에 되도록 하려면.csproj 파일을 확인할 수 있습니다의 모든 항목 `MtouchUseRefCounting` 로 설정 되어 `true`처럼 아래:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

로 설정 된 경우 `false` 응용 프로그램이 새 도구를 사용 하지 않는 합니다.

### <a name="using-older-versions-of-xamarin"></a>이전 버전의 Xamarin 사용 하 여

Xamarin.iOS 7.2.1 및 위의 시스템을 계산 하는 새 참조의 향상 된 미리 보기 기능입니다.

**클래식 API:**

사용 하도록 설정 하려면이 새 참조 횟수를 확인 합니다 **참조 계산 확장 사용** 확인란에는 **고급** 프로젝트 탭 **iOS 빌드 옵션** 를 아래와 같이: 

[![](newrefcount-images/image1.png "새 참조 횟수 시스템을 사용 하도록 설정")](newrefcount-images/image1.png#lightbox)

Mac 용 Visual Studio의 최신 버전에서 이러한 옵션이 제거 되었습니다는 참고

 **[Unified API:](~/cross-platform/macios/unified/index.md)**

 새 참조 계산 확장 Unified API에 대 한 필수 이며 기본적으로 사용 하도록 설정 해야 합니다. IDE의 이전 버전에서이 값이 자동으로 선택 되어 없을 수 있습니다 하 고 확인란을 선택 하 여 직접 할 수도 있습니다.

    
> [!IMPORTANT]
> 이 기능은 이전 버전 이후 주위 MonoTouch 5.2 하지만 사용할 수 있습니다 **sgen** 는 실험적 미리 보기로 있습니다. 이 새로운 향상 된 버전도 출시 되었습니다에 대 한 합니다 **Boehm** 가비지 수집기입니다.


지금까지 있었는지 Xamarin.iOS를 통해 관리 되는 개체의 두 종류:는 네이티브 개체 (피어 개체) 및 확장 하거나 새로운 기능 (파생 된 개체)를 통합 하는 것에 대 한 래퍼로 단순히 일반적으로 추가 메모리 내 상태를 유지 하 여 합니다. 가능한 상태를 사용 하 여 피어 개체를를 보강할 수에서는 이전에 (추가 하 여 예를 들어는 C# 이벤트 처리기) 하지만 해당 개체 참조 되지 않은 이동한 다음 수집 하도록 합니다. 나중에 충돌이 발생할 수 있습니다 (예: Objective-c 런타임에서 관리 되는 개체에 다시 호출 하는 경우).

새 시스템 추가 정보를 저장 하는 경우 런타임에서 관리 되는 개체로 피어 개체를 자동으로 업그레이드 합니다.

이 이와 같은 상황에서 발생 하는 다양 한 충돌을 해결 되었습니다.

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

참조 개수 확장 하지 않고이 코드는 때문에 충돌 `cell` 수집 가능한, 됩니다 등 해당 `TouchDown` 대리자는 현 수 포인터 변환 됩니다.

참조 개수 확장 제공 네이티브 코드의 네이티브 개체를 유지 관리 되는 개체 유지 및 해당 컬렉션을 방지를 보장 합니다.

새 시스템에는 또한에 대 한 필요가 없습니다 *대부분의* 개인 인스턴스를 유지 하는 기본 방법이 있는 바인딩-사용 되는 필드를 백업 합니다. 관리 되는 링커는 모든 것은 제거 하지 못합니다 *불필요 한* 새 참조를 사용 하 여 응용 프로그램에서 필드 계산 확장 합니다.

즉, 각 관리 되는 개체 인스턴스에 이전 보다 적은 메모리를 사용 하는 것을 의미 합니다. 또한 일부 지원 필드는 보면 하는 일부 메모리를 회수 하기 어려운 Objective-c 런타임에서 더 이상 필요 하지 않은 참조 관련된 문제가 해결 되었습니다.
