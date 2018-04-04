---
title: 응용 프로그램 지역화 및 문자열 리소스
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/30/2017
ms.openlocfilehash: cfb127500f919b61788087465700dfed213d5eb2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="application-localization-and-string-resources"></a>응용 프로그램 지역화 및 문자열 리소스

응용 프로그램의 지역화가 대체 리소스를 특정 지역 또는 로캘 대상 제공 하는 작업입니다. 예를 들어 여러 국가 대 한 지역화 된 언어 문자열을 제공할 수 있습니다 또는 색 이나 특정 문화권에 맞게 레이아웃을 변경할 수 있습니다. Android을 로드 하 고 소스 코드에는 별다른 변경 없이 런타임 시 장치의 로캘에 대 한 적절 한 리소스를 사용 합니다.

예를 들어 아래 이미지를 같은 응용 프로그램을 세 개의 다른 장치 로캘에서 실행 보여주지만 각 단추에 표시 되는 텍스트는 각 장치에 설정 된 로캘에도 특정:

[![세 개의 서로 다른 로캘의 예](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

이 예제에서는 레이아웃 파일의 내용 **Main.axml** 다음과 같습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent"
   >
<Button  
   android:id="@+id/myButton"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
android:text="@string/hello"
   />
</LinearLayout>
```

위의 예에서 단추에 대 한 문자열 문자열에 대 한 리소스 ID를 제공 하 여 리소스에서 로드 되었습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![세 가지 언어에 대 한 리소스 문자열](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![세 가지 언어에 대 한 리소스 문자열](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Android 앱 지역화

읽기는 [지역화 소개](~/cross-platform/app-fundamentals/localization.md) 팁과 모바일 앱 지역화에 대 한 지침입니다.

[Android 앱 지역화](~/android/app-fundamentals/localization.md) 가이드에 문자열 번역 및 Xamarin.Android를 사용 하 여 이미지 필드를 지역화 하는 방법에 더 구체적인 예제가 포함 되어 있습니다.



## <a name="related-links"></a>관련 링크

- [Android 앱 지역화](~/android/app-fundamentals/localization.md)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
