---
title: Xamarin.ios의 새 참조 계산 시스템
description: 이 문서에서는 기본적으로 모든 Xamarin.ios 응용 프로그램에서 사용할 수 있는 Xamarin의 향상 된 참조 계산 시스템에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 11/25/2015
ms.openlocfilehash: 56e35662230a3c529eb48a0ae742c2b063c1ac10
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753339"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Xamarin.ios의 새 참조 계산 시스템

Xamarin.ios 9.2.1는 기본적으로 모든 응용 프로그램에 향상 된 참조 횟수 체계를 도입 했습니다. 이전 버전의 Xamarin.ios에서 추적 하 고 수정 하기 어려운 많은 메모리 문제를 제거 하는 데 사용할 수 있습니다.

## <a name="enabling-the-new-reference-counting-system"></a>새 참조 횟수 시스템 사용

Xamarin 9.2.1을 기준으로 새 ref 가산 시스템은 기본적으로 **모든** 응용 프로그램에 대해 사용 하도록 설정 됩니다.

기존 응용 프로그램을 개발 하는 경우 .csproj 파일을 확인 하 여 다음과 같이 모든 항목이 `MtouchUseRefCounting` 로 `true`설정 되었는지 확인할 수 있습니다.

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

로 `false` 설정 된 경우 응용 프로그램은 새 도구를 사용 하지 않습니다.

### <a name="using-older-versions-of-xamarin"></a>이전 버전의 Xamarin 사용

Xamarin.ios 7.2.1 이상에서는 새 참조 횟수 시스템의 향상 된 미리 보기를 제공 합니다.

**Classic API:**

이 새 참조 계산 시스템을 사용 하도록 설정 하려면 아래와 같이 프로젝트의 **IOS 빌드 옵션**의 **고급** 탭에 있는 **참조 계산 확장 사용** 확인란을 선택 합니다. 

[![](newrefcount-images/image1.png "새 참조 계산 시스템 사용")](newrefcount-images/image1.png#lightbox)

이러한 옵션은 최신 버전의 Mac용 Visual Studio에서 제거 되었습니다.

 **[Unified API:](~/cross-platform/macios/unified/index.md)**

 새 참조 계산 확장 프로그램은 Unified API에 필요 하며 기본적으로 사용 하도록 설정 해야 합니다. 이전 버전의 IDE에서는이 값을 자동으로 확인 하지 못할 수 있으며 직접 검사를 수행 해야 할 수도 있습니다.

> [!IMPORTANT]
> 이 기능의 이전 버전은 Monotouch.dialog 5.2부터 발생 했지만 **sgen** 에는 실험적 미리 보기로만 제공 되었습니다. 이제이 새로운 향상 된 버전을 **Boehm** 가비지 수집기 에서도 사용할 수 있습니다.

지금까지 Xamarin.ios에서 관리 하는 두 가지 종류의 개체가 있습니다. 즉, 네이티브 개체 (피어 개체)에 대 한 래퍼입니다 .이는 단순히 추가 메모리 내 상태를 유지 하 여 새 기능 (파생 개체)을 확장 하거나 통합 하는 개체입니다. 이전에는 C# 이벤트 처리기를 추가 하는 등의 상태를 사용 하 여 피어 개체를 확대할 수도 있지만 개체를 참조 하지 않고 수집 하는 것을 허용 했습니다. 이렇게 하면 나중에 충돌이 발생할 수 있습니다 (예: 목표-C 런타임이 관리 되는 개체로 다시 호출 된 경우).

새 시스템은 피어 개체를 추가 정보를 저장할 때 런타임에 의해 관리 되는 개체로 자동으로 업그레이드 합니다.

이렇게 하면 다음과 같은 상황에서 발생 하는 다양 한 충돌이 해결 됩니다.

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

참조 횟수 확장을 사용 하지 않으면이 코드는 `cell` 수집 될 수 있으므로이 코드 `TouchDown` 는 중단 될 수 있으므로 현 수 포인터로 변환 됩니다.

참조 횟수 확장은 네이티브 개체가 네이티브 코드에 의해 유지 되는 경우 관리 되는 개체가 활성 상태로 유지 되 고 해당 컬렉션을 방지 합니다.

또한 새 시스템은 인스턴스를 활성 상태로 유지 하는 기본 방법인 바인딩에 사용 되는 *대부분* 의 전용 지원 필드에 대 한 필요성을 제거 합니다. 관리 되는 링커는 새 참조 횟수 확장을 사용 하 여 응용 프로그램에서 이러한 *불필요* 한 필드를 모두 제거할 수 있습니다.

이는 관리 되는 각 개체 인스턴스가 이전 보다 메모리를 적게 사용 함을 의미 합니다. 또한 일부 지원 필드에서 더 이상 필요 하지 않은 참조를 저장 하는 관련 문제를 해결 하 여 일부 메모리를 더 이상 회수할 수 있습니다.
