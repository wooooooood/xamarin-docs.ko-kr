---
title: 애플리케이션 지역화 및 문자열 리소스
ms.prod: xamarin
ms.assetid: 374A9DA6-1853-8B98-6954-7FE3F591C07C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 45fe5c783e737fb913730082841e0dfafc555684
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755128"
---
# <a name="application-localization-and-string-resources"></a>애플리케이션 지역화 및 문자열 리소스

응용 프로그램 지역화는 특정 지역 또는 로캘을 대상으로 하는 대체 리소스를 제공 하는 행위입니다. 예를 들어 다양 한 국가에 지역화 된 언어 문자열을 제공 하거나 특정 문화권에 맞게 색 또는 레이아웃을 변경할 수 있습니다. Android는 소스 코드를 변경 하지 않고 런타임 시 장치 로캘에 적절 한 리소스를 로드 하 여 사용 합니다.

예를 들어 아래 이미지는 세 개의 서로 다른 장치 로캘로 실행 되는 동일한 응용 프로그램을 보여 주지만 각 단추에 표시 되는 텍스트는 각 장치가로 설정 된 로캘과 관련이 있습니다.

[![세 가지 로캘의 예](application-localization-images/01-click-me-sml.png)](application-localization-images/01-click-me.png#lightbox)

이 예제에서 레이아웃 파일의 내용 ( **Main. axml** )은 다음과 같습니다.

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

위의 예제에서 문자열에 대 한 리소스 ID를 제공 하 여 단추에 대 한 문자열을 리소스에서 로드 했습니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![3 개 언어에 대 한 리소스 문자열](application-localization-images/02-resource-strings-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![3 개 언어에 대 한 리소스 문자열](application-localization-images/02-resource-strings-xs.png)

-----

## <a name="localizing-android-apps"></a>Android 앱 지역화

모바일 앱 지역화에 대 한 팁과 지침은 [지역화 소개](~/cross-platform/app-fundamentals/localization.md) 를 참조 하세요.

[Android 앱 지역화](~/android/app-fundamentals/localization.md) 가이드에는 xamarin.ios를 사용 하 여 문자열을 변환 하 고 이미지를 지역화 하는 방법에 대 한 보다 구체적인 예제가 포함 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 앱 지역화](~/android/app-fundamentals/localization.md)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
