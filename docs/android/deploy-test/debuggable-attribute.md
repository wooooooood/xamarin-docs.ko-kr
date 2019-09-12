---
title: 디버깅 가능한 특성
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/05/2018
ms.openlocfilehash: 10c66e0972c51bba04638706586793a848949980
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754191"
---
# <a name="debuggable-attribute"></a>디버깅 가능한 특성

Android를 디버깅이 가능하도록 JDWP(Java Debug Wire Protocol)를 지원합니다. 이것은 ADB 같은 도구가 JVM과 통신할 수 있게 하는 기술입니다. JDWP는 개발 시 중요하지만 응용 프로그램을 게시하기 전에 비활성화해야 합니다.

JDWP는 Android 애플리케이션에서 `android:debuggable` 특성 값이 될 수 있습니다. Xamarin.Android는 이러한 특성을 설정할 수 있는 다음과 같은 방법을 제공합니다.

1. `AndroidManifext.xml` 파일을 만들어 `android:debuggable` 특성 설정.
2. `.CS` 파일에 `ApplicationAttribute` 포함(예: `[assembly: Application(Debuggable=false)]`).

`AndroidManifest.xml` 및 `ApplicationAttribute`가 둘 다 있을 경우 `AndroidManifest.xml`의 콘텐츠가 `ApplicationAttribute`에서 지정하는 것보다 우선합니다.

`AndroidManifest.xml` 또는 `ApplicationAttribute`가 둘 다 없을 경우 `android:debuggable` 특성의 기본값은 디버그 기호의 생성 여부에 따라 달라집니다. 디버그 기호가 있을 경우 Xamarin.Android가 `android:debuggable` 특성을 `true`로 설정합니다.

`android:debuggable` 특성 값이 반드시 빌드 구성에 따라 달라지는 것은 아닙니다. 릴리스 빌드는 `android:debuggable` 특성이 true로 설정되어 있을 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Android Market에서 디버그 가능한 앱](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
