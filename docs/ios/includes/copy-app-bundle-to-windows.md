---
ms.openlocfilehash: 22a8542c0e829db8b889abc2c7fe5f5ab9d19ed4
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527631"
---

Visual Studio 및 Mac 빌드 에이전트에서 iOS 앱을 빌드하는 경우 .app 번들이 Windows 컴퓨터에 다시 복사되지 않습니다. Visual Studio 7.4용 Xamarin Tools는 CI가 .app 번들을 Windows로 다시 복사하도록 해주는 새로운 `CopyAppBundle` 속성을 추가합니다.

이 기능을 사용하려면 이 기능을 적용하려는 속성 그룹에서 .csproj에 `CopyAppBundle` 속성을 추가합니다. 예를 들어 다음 예제는 **iPhoneSimulator**를 대상으로 하는 **디버그** 빌드를 위해 .app 번들을 Windows 컴퓨터에 다시 복사하는 방법을 보여줍니다.

```xml
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">
    <CopyAppBundle>true</CopyAppBundle>
</PropertyGroup>
```
