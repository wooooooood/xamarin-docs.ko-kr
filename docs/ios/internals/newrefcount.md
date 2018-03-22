---
title: "새 참조 계산 시스템"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 723a9c4a052f7f432ba0f32ec501af3221b2696f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="new-reference-counting-system"></a>새 참조 계산 시스템

Xamarin.iOS 9.2.1 도입 향상 된 참조 기본적으로 모든 응용 프로그램에는 시스템을 계산 합니다. 메모리 문제를 추적 하 고 이전 버전의 Xamarin.iOS 수정할 어려웠습니다 해결할를 사용할 수 있습니다.

## <a name="enabling-the-new-reference-counting-system"></a>새 참조 가산 시스템을 사용 하도록 설정

Xamarin 9.2.1 기준으로 새 ref 시스템을 계산 하기 위해 설정 된 **모든** 기본적으로 응용 프로그램입니다.

기존 응용 프로그램을 개발 하는 경우에 되도록.csproj 파일을 확인할 수 있습니다의 항목을 모두 `MtouchUseRefCounting` 로 설정 `true`처럼 아래:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

로 설정 되어 있으면 `false` 응용 프로그램 새 도구를 사용 하지 것입니다.

### <a name="using-older-versions-of-xamarin"></a>이전 버전의 Xamarin 사용 하 여

Xamarin.iOS 7.2.1 및 위에 새 참조 시스템 횟수의 향상 된 미리 보기 기능입니다.

**클래식 API:**

이 새 참조 계산 시스템을 사용 하도록 설정 하려면 확인는 **참조 가산 확장을 사용 하 여** 확인란에서 찾을 수는 **고급** 프로젝트의 탭 **iOS 빌드 옵션** 아래와 같이: 

[![](newrefcount-images/image1.png "새 참조 계산 시스템을 사용 하도록 설정")](newrefcount-images/image1.png#lightbox)

이러한 옵션 Mac.에 대 한 최신 버전의 Visual Studio에서 제거 된 있는지 참고

 **[통합된 API:](~/cross-platform/macios/unified/index.md)**

 확장을 계산 하 고 새 참조는 통합 API에 필요 하며 기본적으로 사용 하도록 설정 합니다. 이전 버전의 IDE에 자동으로 확인 하는이 값을 사용할 수 없습니다 및 확인란에 의해 직접 해야 할 수 있습니다.

    
> [!IMPORTANT]
> 이 기능은 이전 버전은 MonoTouch 5.2 하지만 사용할 수에 대 한 이후 되었습니다 **sgen** 실험적 미리 보기로 합니다. 이 새로운 향상 된 버전도 수 이제는 **Boehm** 가비지 수집기입니다.


지금까지 있었는지 Xamarin.iOS에서 관리 하는 두 가지: 추가 메모리 상태를 유지 하 여 네이티브 개체 (피어 개체)과 확장 하거나 새로운 기능 (파생된 개체)를 포함 하는에 대 한 래퍼로 단순히 일반적으로 된 합니다. 이전에 있었습니다 (예를 추가 하 여 C# 이벤트 처리기) 상태와 피어 개체를 보강할 수 म 있 참조 되지 않은 이동한 후 수집 된 개체를 사용 했습니다. 나중에 충돌이 발생할 수 (예: 관리 되는 개체로 Objective C 런타임 다시 호출 하는 경우).

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

참조 개수 확장 없이이 코드는 때문에 충돌 `cell` 는 수집 가능한, 사용 하 고 있어서 해당 `TouchDown` 위임 현 수 포인터에는 변환입니다.

참조 개수 확장 하면 관리 되는 개체 연결이 유지 되는 해당 컬렉션이 않도록 네이티브 개체는 네이티브 코드에 의해 보존 된 제공.

새 시스템에 대 한 필요성 제거 *대부분* 개인 바인딩으로-는 인스턴스를 유지 하는 기본 방법에서 사용 되는 필드를 백업 합니다. 관리 되는 링커는 모두 제거 하 여 *불필요 한* 필드 새 참조를 사용 하 여 응용 프로그램에서 확장을 계산 합니다.

즉, 각 관리 되는 개체 인스턴스가 하기 전에 보다 적은 메모리를 사용 합니다. 또한 일부 지원 필드 보이게 일부 메모리를 회수할 Objective-c 런타임에서 더 이상 필요 하지 않은 참조를 저장 해야 하는 관련된 문제를 해결 합니다.
