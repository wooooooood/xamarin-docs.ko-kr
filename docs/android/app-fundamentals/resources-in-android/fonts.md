---
title: 글꼴
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/13/2018
ms.openlocfilehash: 086576ea7d806bb0768fbe4563df7fca99244ccb
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044823"
---
# <a name="fonts"></a>글꼴

## <a name="overview"></a>개요

리소스는 레이아웃 마찬가지로 형태로 처리 되는 글꼴 사용 하면 Android SDK API 레벨 26 부터는 또는 drawables 합니다. [Android 지원 라이브러리 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) backport API 수준 14 이상을 대상으로 하는 응용 프로그램을 새 글꼴 API의 됩니다.

대상으로 하는 API 26 또는 Android 지원 라이브러리 v26 설치 후 Android 응용 프로그램에서 글꼴을 사용 하는 방법은 두 가지 있습니다.

1. **Android 리소스로 글꼴 패키지** &ndash; 하면 글꼴은 항상 응용 프로그램에 사용할 수 있지만 APK의 크기가 늘어납니다. 
2. **글꼴 다운로드** &ndash; Android에서 글꼴 다운로드도 지원는 _글꼴 공급자_합니다. 글꼴 공급자는 장치에 이미 글꼴을 적용 하는 경우를 확인 합니다. 필요에 따라 글꼴을 다운로드 하 고 장치에 캐시 됩니다. 이 글꼴은 여러 응용 프로그램 간에 공유할 수 있습니다.

비슷한 글꼴 (또는 몇 가지 다른 스타일을 줄 수 있는 글꼴)으로 그룹화 할 수 있지만 _글꼴 패밀리_합니다. 이렇게 하면 개발자의 가중치 등의 글꼴의 특정 한 특성을 지정할 수 있고 Android 적절 한 글꼴의 글꼴 패밀리에서 자동으로 선택 합니다.

Android 지원 라이브러리 v26 API 수준 26 글꼴 backport 지원이 됩니다. 이 이전 API 레벨을 대상으로 지정 하는 경우를 선언 해야는 `app` XML 네임 스페이스를 생성 하 고 사용 하 여 다양 한 글꼴 특성 이름을 지정 하는 `android:` 네임 스페이스 및 `app:` 네임 스페이스입니다. 경우에는 `android:` 글꼴을 25 또는 API 레벨을 실행 하는 표시 장치 됩니다. 다음 네임 스페이스가 사용 됩니다. 예를 들어 다음 XML 조각을 선언 새 [ _글꼴 패밀리_ ](#font_families) API 수준 14 이상에서 작동 하는 리소스:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular" 
            android:fontStyle="normal" 
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular" 
            app:fontStyle="normal" 
            app:fontWeight="400" />

</font-family>
```

글꼴 Android 응용 프로그램에 적절 한 방식으로 제공 됩니다,으로 수에 적용 될 있습니다 UI 위젯 설정 하 여는 [ `fontFamily` 특성](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)합니다. 예를 들어 다음 코드 조각은 TextView의 글꼴을 표시 하는 방법을 보여 줍니다.

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

이 가이드는 먼저 Android 리소스로 글꼴을 사용 하는 방법에 설명 하 고 런타임 시 글꼴을 다운로드 하는 방법에 토론 하려면 이동 합니다.

## <a name="fonts-as-a-resource"></a>글꼴 리소스

Android APK에 글꼴을 패키징 응용 프로그램에 사용할 수 있는 항상 되는지 확인 합니다. 글꼴 파일 (중 하나는 합니다. TTF 또는 합니다. OTF 파일이)에 하위 디렉터리에 파일을 복사 하 여 다른 리소스와 마찬가지로 Xamarin.Android 응용 프로그램에 추가 되는 **리소스** Xamarin.Android 프로젝트의 폴더입니다. 글꼴 리소스에 보관 되는 **글꼴** 의 하위 디렉터리는 **리소스** 프로젝트의 폴더입니다. 

> [!NOTE]
> 글꼴 있어야는 **빌드 작업** 의 **AndroidResource** 또는 최종 APK에 패키지 되지 것입니다. 빌드 작업에서 IDE에 의해 자동으로 설정 되어야 합니다.

유사한 여러 글꼴 파일 (예를 들어 서로 다른 가중치 또는 스타일을 사용 하 여 동일한 모양을) 경우에 글꼴 패밀리에 그룹화 할 수 있습니다.

<a name="font_families" />

### <a name="font-families"></a>글꼴 패밀리

글꼴 패밀리는 서로 다른 가중치 및 스타일은 글꼴의 집합입니다. 예를 들어 굵게, 기울임꼴 글꼴에 대 한 별도 글꼴 파일이 있을 수 있습니다. 글꼴 패밀리 정의한 `font` 에 유지 되는 XML 파일의 요소는 **리소스/글꼴** 디렉터리입니다. 각 글꼴 패밀리 자신의 XML 파일에 있어야 합니다.

만들려는 글꼴 패밀리에 있는 모든 글꼴을 먼저 추가 **리소스/글꼴** 폴더. 그런 다음 글꼴 패밀리에 대 한 글꼴 폴더에 새 XML 파일을 만듭니다. XML 파일의 이름에; 참조 되는 글꼴에 선호도 또는 관계 없습니다. 리소스 파일에 모든 올바른 Android 리소스 파일 이름일 수 있습니다. 이 XML 파일에는 루트 갖습니다 `font-family` 하나 이상 포함 된 요소 `font` 요소입니다. 각 `font` 요소 글꼴의 특성을 선언 합니다.

다음 XML은 글꼴 패밀리에 대 한 예는 _소스 San Pro_ 많은 다른 글꼴 두께 정의 하는 글꼴입니다. 이 파일에로 저장 됩니다는 **리소스/글꼴** 라는 폴더 **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular" 
          android:fontStyle="normal" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular" 
          app:fontStyle="normal" 
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold" 
          android:fontStyle="normal" 
          android:fontWeight="800" 
          app:font="@font/sourcesanspro_bold" 
          app:fontStyle="normal" 
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic" 
          android:fontStyle="italic" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic" 
          app:fontStyle="italic" 
          app:fontWeight="400" />
</font-family>
```

`fontStyle` 특성에 두 개의 가능한 값:

* **일반** &ndash; 일반 글꼴
* **기울임꼴** &ndash; 기울임꼴 글꼴

`fontWeight` CSS에 해당 하는 특성 `font-weight` 특성 및 글꼴의 두께 가리킵니다. 100-900 범위의 값입니다. 다음 목록에서는 일반적인 글꼴 두께 값 및 해당 이름을 설명합니다.

* **씬** &ndash; 100
* **특 밝은** &ndash; 200
* **밝은** &ndash; 300
* **보통** &ndash; 400
* **보통** &ndash; 500
* **굵게 semi** &ndash; 600
* **굵게** &ndash; 700
* **T** &ndash; 800
* **블랙** &ndash; 900

설정 하 여 선언적으로 사용할 수 있습니다 글꼴 패밀리를 정의 고 `fontFamily`, `textStyle`, 및 `fontWeight` 레이아웃 파일의 특성입니다.  예를 들어 다음 XML 조각 (일반) 글꼴 두께 400과 기울임꼴 텍스트 스타일 설정합니다.

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### <a name="programmatically-assigning-fonts"></a>글꼴을 프로그래밍 방식으로 할당

글꼴을 사용 하 여 프로그래밍 방식으로 설정할 수 있습니다는 [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) 를 검색할 메서드는 [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) 개체입니다. 보기에 많은 `TypeFace` 글꼴 위젯에 할당을 사용할 수 있는 속성입니다. 이 코드 조각은 프로그래밍 방식으로 TextView의 글꼴을 설정 하는 방법을 보여 줍니다.

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont` 메서드 자동으로 로드 합니다는 글꼴 패밀리 내에서 첫 번째 글꼴입니다.  사용 하 여 특정 스타일과 일치 하는 글꼴을 로드 하려면는 `Typeface.Create` 메서드. 이 메서드는 지정된 된 스타일을 일치 하는 글꼴을 로드 하려고 합니다. 예를 들어이 조각은 굵게 표시 된 로드를 시도 합니다 `Typeface` 개체에 정의 된 글꼴 패밀리를 **리소스/글꼴**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>글꼴 다운로드

응용 프로그램 리소스 패키징 글꼴는 대신 Android 원격 소스에서 글꼴을 다운로드할 수 있습니다. 이 APK의 크기를 줄이면의 바람직한 효과 갖습니다. 

글꼴의 도움으로 다운로드 되는 _글꼴 공급자_합니다. 다운로드와 장치에서 모든 응용 프로그램에 대 한 글꼴의 캐싱을 관리 하는 특수 한 콘텐츠 공급자입니다. Android 8.0에서 글꼴을 다운로드 하려면 글꼴 공급자가 포함 되어는 [Google 글꼴 리포지토리](http://fonts.google.com)합니다. 이 기본 글꼴 공급자는 backported Android 지원 라이브러리 v26와 API 수준 14입니다.

글꼴에 대 한 요청을 수행 하는 응용 프로그램, 글꼴 공급자는 먼저 글꼴이 이미 장치에 있는지 확인 합니다. 그렇지 않은 경우 글꼴 다운로드를 시도 합니다. 다운로드 한, 다음 Android 글꼴 수 없는 경우 기본 시스템 글꼴을 사용 합니다. 글꼴 다운로드 되 면이 초기 요청을 만든 응용 프로그램 뿐 아니라 장치에서 모든 응용 프로그램에 사용할 수 있습니다.

에 글꼴을 다운로드 하는 요청이 수행 될 때 앱 글꼴 공급자를 직접 쿼리할 하지 않습니다. 앱은의 인스턴스를 사용 하는 대신,는 [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (또는 [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) 지원 라이브러리 26 사용 되는 경우).  

Android 8.0 두 가지 방법으로 다운로드 글꼴을 지원합니다.

1. **리소스로 다운로드 글꼴 선언** &ndash; 응용 프로그램 XML 리소스 파일을 통해 Android 다운로드할 수 있는 글꼴을 선언할 수 있습니다. 이러한 파일에는 Android 앱을 시작 하 고 장치에 캐시 하는 글꼴을 비동기적으로 다운로드 하는 메타 데이터를 모두 포함 됩니다.
2. **프로그래밍 방식으로** &ndash; 26 Android API 수준에 있는 Api를 사용 하는 응용 프로그램을 실행 하는 동안 글꼴을 프로그래밍 방식으로 다운로드 하는 응용 프로그램입니다. 앱을 만듭니다는 `FontRequest` 지정된 된 글꼴에 대 한 개체를 전달 하기 위해이 개체는 `FontsContract` 클래스입니다. `FontsContract` 사용는 `FontRequest` 에서 글꼴을 검색 하 고는 _글꼴 공급자_합니다. Android 글꼴을 동기적으로 다운로드 합니다. 만드는 예는 `FontRequest` 이 가이드의 뒷부분에 나오는 표시 됩니다.

응용 프로그램이 사용 되는 어떤 방법에 관계 없이 글꼴 전에 Xamarin.Android 응용 프로그램에 추가 해야 하는 리소스 파일을 다운로드할 수 있습니다. 글꼴의 XML 파일에 선언 해야 먼저는 **리소스/글꼴** 글꼴 패밀리의 일부로 디렉터리입니다. 이 코드 조각에서 글꼴을 다운로드 하는 방법의 한 예로 [Google 글꼴 오픈 소스 컬렉션](https://fonts.google.com) Android 8.0 (또는 지원 라이브러리 v26)와 함께 제공 되는 기본 글꼴 공급자를 사용 하 여:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts" 
             android:fontProviderPackage="com.google.android.gms" 
             android:fontProviderQuery="VT323" 
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts" 
             app:fontProviderPackage="com.google.android.gms" 
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

`font-family` 요소 선언에서 필요한 정보를 Android 글꼴을 다운로드 하는 다음 특성을 포함 합니다.

1. **fontProviderAuthority** &ndash; 요청에 사용할 글꼴 공급자의 인증 기관입니다.
2. **fontPackage** &ndash; 요청에 사용할 글꼴 공급자에 대 한 패키지입니다. 공급자의 id를 확인 하는이 사용 됩니다.
3. **fontQuery** &ndash; 글꼴 공급자 요청 된 글꼴을 찾는 데 도움이 되는 문자열입니다. 글꼴 쿼리에 대 한 세부 정보에서 글꼴 공급자에 적용 됩니다. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) 클래스에 [다운로드 글꼴](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) 샘플 응용 프로그램 일부 정보를 제공 하는 쿼리 형식에 글꼴에 대 한 Google 글꼴 열기 소스 컬렉션에서 합니다.
4. **fontProviderCerts** &ndash; 공급자도 서명 되어야 하는 인증서에 대 한 해시 집합의 목록 사용 하 여 리소스 배열입니다.

글꼴을 정의한 후에 대 한 정보를 제공 해야 할 수 있습니다는 _글꼴 인증서_ 다운로드에 포함 합니다.

### <a name="font-certificates"></a>글꼴 인증서

글꼴 공급자는 장치에 사전 설치 되지 않은 경우 또는 응용 프로그램을 사용 하는 경우는 `Xamarin.Android.Support.Compat` 라이브러리, Android의 글꼴 공급자 보안 인증서가 필요 합니다. 이러한 인증서에 유지 되는 배열 리소스 파일에 나열 됩니다 **리소스/값** 디렉터리입니다. 

예를 들어 다음 XML 라는 **Resources/values/fonts_cert.xml** Google 글꼴 공급자에 대 한 인증서를 저장 합니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

이러한 리소스는 위치에 파일을 통해 앱이 글꼴을 다운로드할 수 있습니다.

### <a name="declaring-downloadable-fonts-as-resources"></a>리소스 그룹으로 다운로드할 수 있는 글꼴 선언

다운로드 가능한 글꼴을 나열 하 여는 **AndroidManifest.XML**, 앱을 처음 시작할 때 Android 글꼴을 비동기적으로 다운로드 됩니다. 글꼴의 자체는이 이와 유사한 배열 리소스 파일에 나열 됩니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

에 선언 해야이 글꼴을 다운로드 하려면 **AndroidManifest.XML** 추가 하 여 `meta-data` 의 자식으로는 `application` 요소입니다. 다운로드 가능한 글꼴에 있는 리소스 파일에 선언 된 경우 등 **Resources/values/downloadable_fonts.xml**,이 코드 조각이 매니페스트에 추가할 되었으면: 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>글꼴 Api가 있는 글꼴 다운로드

프로그래밍 방식으로 인스턴스화하여 글꼴을 다운로드할 수는 [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) 개체와 전달 되는 `FontContractCompat.RequestFont` 메서드. `FontContractCompat.RequestFont` 메서드는 먼저 글꼴의 장치에 있는지 확인 한 다음 필요한 경우는 비동기적으로 쿼리 공급자 글꼴 및 글꼴 응용 프로그램을 다운로드 하려고 합니다. 경우 `FontRequest` 하지 못하면 해당 글꼴을 다운로드 Android 기본 시스템 글꼴을 사용 합니다. 

A `FontRequest` 찾고 글꼴을 다운로드 하는 글꼴 공급자에서 사용할 수 있는 정보를 포함 하는 개체입니다. A `FontRequest` 네 가지 정보가 필요 합니다.

1. **글꼴 공급자 기관** &ndash; 요청에 사용할 글꼴 공급자의 인증 기관입니다.
2. **글꼴 패키지** &ndash; 요청에 사용할 글꼴 공급자에 대 한 패키지입니다. 공급자의 id를 확인 하는이 사용 됩니다.
3. **글꼴 쿼리** &ndash; 글꼴 공급자 요청 된 글꼴을 찾는 데 도움이 되는 문자열입니다. 글꼴 쿼리에 대 한 세부 정보에서 글꼴 공급자에 적용 됩니다. 문자열의 세부 정보에서 글꼴 공급자별으로 다릅니다. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) 클래스에 [다운로드 글꼴](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) 샘플 응용 프로그램 일부 정보를 제공 하는 쿼리 형식에 글꼴에 대 한 Google 글꼴 열기 소스 컬렉션에서 합니다.
4. **글꼴 공급자 인증서** &ndash; 공급자도 서명 되어야 하는 인증서에 대 한 해시 집합의 목록 사용 하 여 리소스 배열입니다. 

이 조각은 새 인스턴스화의 한 예로 `FontRequest` 개체:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

이전 조각에서 `FontToDownload` 는 Google 글꼴 오픈 소스 컬렉션에서 글꼴은 데 도움이 되는 쿼리입니다. 

전달 되기 전에 `FontRequest` 에 `FontContractCompat.RequestFont` 메서드를 만들어야 하는 개체 두 개가:

* **`FontsContractCompat.FontRequestCallback`** &ndash; 이 클래스는 추상 클래스 확장 해야 합니다. 될 콜백은 될 때 호출 `RequestFont` 를 완료 합니다. Xamarin.Android 응용 프로그램 하위 클래스를 해야 합니다. `FontsContractCompat.FontRequestCallback` 재정의 `OnTypefaceRequestFailed` 및 `OnTypefaceRetrieved`, 다운로드에 실패 하거나 각각 성공 하는 경우 수행할 동작을 제공 합니다.
* **`Handler`** &ndash; 이는 `Handler` 에서 사용 됩니다 `RequestFont` 필요한 경우 스레드에서 글꼴을 다운로드 합니다. 글꼴 해야 **하지** UI 스레드에서 다운로드할 수 있습니다.

이 코드 조각에는 Google 글꼴 오픈 소스 컬렉션에서 글꼴을 비동기적으로 다운로드 됩니다 하는 C# 클래스의 예시입니다. 구현 하는 `FontRequestCallback` 인터페이스, 및 C# 이벤트를 발생 시킨 때 `FontRequest` 완료 합니다. 

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

이 도우미를 사용 하려면 새 `FontDownloadHelper` 만들어지면 이벤트 처리기가 할당 됩니다.  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>요약

이 가이드의 다운로드 가능한 글꼴 및 리소스로 글꼴을 지원 하기 위해 Android 8.0 Api를 설명 합니다. 그는 APK에 기존 글꼴을 포함 하는 레이아웃에서이 사용 하는 방법을 설명 합니다. 또한 프로그래밍 방식으로 또는 리소스 파일의 글꼴 메타 데이터를 선언 하 여 Android 8.0 글꼴 공급자에서 다운로드 글꼴을 지원 하는 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [서체](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android 지원 라이브러리 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android에서 글꼴을 사용 하 여](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS 글꼴 두께 사양](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google 글꼴 오픈 소스 컬렉션](https://fonts.google.com/)
- [Pro San 소스](https://fonts.google.com/specimen/Source+Sans+Pro)
