---
title: 디버깅 가능한 특성
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 12/13/2019
ms.openlocfilehash: 6e54dba0c832596818c5163dc4e14ad1e659e040
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487973"
---
# <a name="debuggable-attribute"></a>디버깅 가능한 특성

Android를 디버깅이 가능하도록 JDWP(Java Debug Wire Protocol)를 지원합니다. 이것은 ADB 같은 도구가 JVM과 통신할 수 있게 하는 기술입니다. JDWP는 개발 시 중요하지만, 애플리케이션을 게시하기 전에 비활성화해야 합니다.

JDWP는 Android 애플리케이션에서 `android:debuggable` 특성 값으로 구성할 수 있습니다. 다음 세 가지 방법 중 _하나_를 선택하여 Xamarin.Android에서 이 특성을 설정합니다.

## <a name="androidmanifestxml"></a>AndroidManifest.xml

`AndroidManifext.xml` 파일을 만들거나 열고 `android:debuggable` 특성을 설정합니다. 디버깅이 사용 설정된 상태로 릴리스 빌드를 제공하지 않도록 주의해야 합니다.

## <a name="add-an-application-class-attribute"></a>애플리케이션 클래스 특성 추가

Xamarin.Android 앱에 `[Application]` 특성이 포함된 클래스가 있는 경우 특성을 `[Application(Debuggable = true)]`(으)로 업데이트합니다. 사용 중지하려면 `false`으(로) 설정합니다.

## <a name="add-an-assembly-attribute"></a>어셈블리 특성 추가

Xamarin.Android 앱에 `[Application]` 클래스 특성이 아직 없는 경우 c# 파일에 어셈블리 수준 특성 `[assembly: Application(Debuggable=true)]`을(를) 추가합니다. 사용 중지하려면 `false`으(로) 설정합니다.

## <a name="summary"></a>요약

`AndroidManifest.xml` 및 `ApplicationAttribute`가 둘 다 있을 경우 `AndroidManifest.xml`의 콘텐츠가 `ApplicationAttribute`에서 지정하는 것보다 우선합니다.

클래스 특성 _및_ 어셈블리 특성을 모두 추가하는 경우 컴파일러 오류가 발생합니다.

```error
"Error The "GenerateJavaStubs" task failed unexpectedly.
System.InvalidOperationException: Application cannot have both a type with an [Application] attribute and an [assembly:Application] attribute."
```

기본적으로 `AndroidManifest.xml` 또는 `ApplicationAttribute` 모두 없을 경우 `android:debuggable` 특성의 값은 디버그 기호의 생성 여부에 따라 달라집니다. 디버그 기호가 있을 경우 Xamarin.Android가 `android:debuggable` 특성을 `true`(으)로 설정합니다.

> [!WARNING]
> `android:debuggable` 특성 값이 반드시 빌드 구성에 따라 달라지는 것은 아닙니다. 릴리스 빌드는 `android:debuggable` 특성이 true로 설정되어 있을 수 있습니다. 특성을 사용하여 이 값을 설정하는 경우 컴파일러 지시문에서 특성을 래핑하도록 선택할 수 있습니다.
> 
> ```csharp
> #if DEBUG
> [Application(Debuggable = true)]
> #else
> [Application(Debuggable = false)]
> #endif
> ```

## <a name="related-links"></a>관련 링크

- [Android Market에서 디버그 가능한 앱](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
