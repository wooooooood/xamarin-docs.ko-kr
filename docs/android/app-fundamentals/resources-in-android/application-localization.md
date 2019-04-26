---
title: 애플리케이션 지역화 및 문자열 리소스
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: d9d90e371199c8587d61199240523cf0a23f5efd
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013379"
---
# <a name="application-localization-and-string-resources"></a>애플리케이션 지역화 및 문자열 리소스

응용 프로그램 지역화는 특정 지역 또는 로캘을 대상으로 대체 리소스를 제공 하는 행위. 예를 들어, 다양 한 국가 대 한 지역화 된 언어 문자열을 제공할 수 있습니다 또는 색 또는 특정 문화권에 맞게 레이아웃을 변경할 수 있습니다. Android는 장치의 로캘에 대 한 적절 한 리소스를 사용 하 여 소스 코드를 변경 하지 않고도 런타임 시을 로드 합니다.

예를 들어, 아래 이미지 동일한 응용 프로그램을 세 가지 다른 장치 로캘에서 실행 보여주지만 각 단추에 표시할 텍스트를 각 장치에 설정 된 로캘을에 따라 다릅니다.

[![세 가지 다른 로캘의 예로](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

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

위의 예제에서는 단추에 대 한 문자열 문자열에 대 한 리소스 ID를 제공 하 여 리소스에서 로드 되었습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![세 가지 언어에 대 한 리소스 문자열](application-localization-images/02-resource-strings-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![세 가지 언어에 대 한 리소스 문자열](application-localization-images/02-resource-strings-xs.png)
 
-----
 
## <a name="localizing-android-apps"></a>Android 앱을 지역화

읽기를 [지역화 소개](~/cross-platform/app-fundamentals/localization.md) 팁과 모바일 앱을 지역화에 대 한 지침입니다.

합니다 [Android 앱 지역화](~/android/app-fundamentals/localization.md) 문자열을 번역 하 고 Xamarin.Android를 사용 하 여 이미지를 지역화 하는 방법에 보다 구체적인 예제를 포함 하는 가이드입니다.



## <a name="related-links"></a>관련 링크

- [Android 앱을 지역화](~/android/app-fundamentals/localization.md)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
