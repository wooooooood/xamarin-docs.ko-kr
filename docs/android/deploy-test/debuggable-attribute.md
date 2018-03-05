---
title: "디버깅 가능한 특성"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>디버깅 가능한 특성

<a name="Overview" />


Android를 디버깅이 가능하도록 JDWP(Java Debug Wire Protocol)를 지원합니다. 이것은 ADB 같은 도구가 JVM과 통신할 수 있게 하는 기술입니다. JDWP는 개발 시 중요하지만 응용 프로그램을 게시하기 전에 비활성화해야 합니다.

JDWP는 Android 응용 프로그램에서 `android:debuggable` 특성 값이 될 수 있습니다. Xamarin.Android는 이러한 특성을 설정할 수 있는 다음과 같은 방법을 제공합니다.

1.  `AndroidManifext.xml` 파일을 만들어 `android:debuggable` 특성 설정.
1.  `.CS` 파일에 `ApplicationAttribute` 포함(예: `[assembly: Application(Debuggable=false)]`).


`AndroidManifest.xml` 및 `ApplicationAttribute`가 둘 다 있을 경우 `AndroidManifest.xml`의 콘텐츠가 `ApplicationAttribute`에서 지정하는 것보다 우선합니다.

`AndroidManifest.xml` 및 `ApplicationAttribute`가 둘 다 없을 경우 `android:debuggable` 특성의 기본값은 디버그 기호의 생성 여부에 따라 달라집니다. 디버그 기호가 있을 경우 Xamarin.Android가 `android:debuggable` 특성을 `true`로 설정합니다.

`android:debuggable` 특성 값이 반드시 빌드 구성에 따라 달라지는 것은 아닙니다. 릴리스 빌드는 `android:debuggable` 특성이 true로 설정되어 있을 수 있습니다.


## <a name="related-links"></a>관련 링크

- [Android Market에서 디버그 가능한 앱](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
